---
title: "Forecasting in SQL: Building an Air Quality Model in BigQuery"
date: 2026-06-11T22:56:00Z
description: "Building and validating an air-quality forecasting model in BigQuery ML—and finding the data leakage that would break it in production."
summary: "Building and validating an air-quality forecasting model in BigQuery ML—and finding the data leakage that would break it in production."
tags: []
showToc: true
TocOpen: false
---
BigQuery ML allows you to build models directly where your data resides. You avoid moving large datasets into a separate training system, and the SQL interface makes it straightforward to prototype a model without maintaining a custom serving stack.

For this project, I tried to predict PM2.5 particulate levels across Delhi using **BigQuery ML (BQML)** and an ARIMA model. The experiment also demonstrates how easily data leakage can sneak into development and make strong validation metrics useless in production.

## Understanding ARIMA

Common predictive models such as boosted trees make predictions from a set of inputs. That works well for problems such as house prices, where relevant inputs might include ZIP code, square footage, and condition.

However, this does not take into account historic data during inference. Imagine you are trying to predict the number of umbrellas sold in a city. Sales spike every time it rains, but you also notice a steady increase every year as the city grows. **ARIMA** (AutoRegressive Integrated Moving Average) uses time as an input to solve this.

**AutoRegressive:** It looks at the past (e.g., “Yesterday was high, so today is likely high”). **Integrated:** It looks at trends (e.g., “The data is slowly drifting upward, so I should account for that”). **Moving Average:** It looks at recent noise (e.g., “Ignore that one-day blip, it’s just bad luck”).

## Building the model

The dataset for this task is available on [Hugging Face](https://huggingface.co/datasets/abhinavsarkar/delhi_air_quality_feature_store_processed.csv). It includes features such as wind speed, pressure, and humidity, along with our target, PM2.5. For preprocessing, I calculated the daily average of each feature.

```sql
CREATE OR REPLACE TABLE `delhiAQI.location_specific_aqi_processed` AS
SELECT
  DATE_TRUNC(DATE(event_timestamp), DAY) as forecast_date,

  -- Aggregated features for this specific location on this specific day
  AVG(pm25) as avg_pm25,
  AVG(pm10) as avg_pm10,
-- ... truncated
GROUP BY ALL
ORDER BY forecast_date, location_id;
```

I decided to go all-in with a multivariate “Kitchen Sink” model. By providing the model with a full suite of environmental variables, I enabled it to understand the physical reality of the city’s air quality. Model creation code follows:

```sql
CREATE OR REPLACE MODEL `arima-aqi.delhiAQI.model_full_kitchen_sink`
OPTIONS(
  MODEL_TYPE = 'ARIMA_PLUS_XREG',
  TIME_SERIES_TIMESTAMP_COL = 'forecast_date',
  TIME_SERIES_DATA_COL = 'avg_pm25',
  TIME_SERIES_ID_COL = 'location_id', -- Trains a model per sensor location
  AUTO_ARIMA = TRUE,                  -- Manage hyperparameter tuning automatically
  CLEAN_SPIKES_AND_DIPS = TRUE        -- Handles outliers in the data
) AS
SELECT
  location_id,
  forecast_date,
  avg_pm25,
  -- Covariates
  avg_wind_speed,
  avg_pm10,
  avg_no2,
  avg_o3,
  avg_co,
  avg_temp,
  avg_humidity,
  avg_pressure,
  avg_wind_direction
FROM `arima-aqi.delhiAQI.location_specific_aqi_processed`
WHERE forecast_date < '2024-08-07'; -- Withholding 90 days of data for validation
```

## Validation

The cardinal rule of validating time-series models is to never randomly shuffle the data. Random splits break temporal dependencies and can allow the model to peek into the future. We need to keep the chronological series intact.

I accomplished this above by withholding the final 90 days of sequential data as a continuous, untouched chronological hold-out set for our validation. To perform the actual validation, we can run a query like this:

```sql
CREATE OR REPLACE TABLE `arima-aqi.delhiAQI.forecast_full_kitchen_sink` AS
SELECT * FROM ML.FORECAST(
  MODEL `arima-aqi.delhiAQI.model_full_kitchen_sink`,
  STRUCT(90 AS horizon),
  (
    SELECT
      location_id,
      forecast_date,
      avg_wind_speed, avg_pm10, avg_no2, avg_o3,
      avg_co, avg_temp, avg_humidity, avg_pressure, avg_wind_direction
    FROM `arima-aqi.delhiAQI.location_specific_aqi_processed`
    WHERE forecast_date >= '2024-08-07' -- use unseen data for validation
  )
);
```

On average, my RMSE across each sensor was less than 3, which indicates a strongly performing model.

## Understanding Data Leakage

As shown above, building and testing time-series models in BigQuery is trivially easy. This entire project took me less than a day. However, in the real world, my validation metrics would be completely useless.

If you think about it, to predict PM2.5, our model relies on features like wind speed, pressure, and temperature. In our historical dataset, this weather data is already sitting there neatly. But in production, you don’t actually know tomorrow’s wind speed or temperature when you are making today’s prediction. This is a classic data leakage trap. While weather is an obvious covariate to spot, in massive systems with thousands of features, it is incredibly easy for data leakage to quietly sneak into your model development.

One production approach would be a **hierarchical pipeline**. First, use simpler models or a weather-forecast source to estimate future meteorological variables such as temperature and wind speed. Then feed those *forecasted* values into the multivariate ARIMA model. The true end-to-end validation score should be calculated across the chained system. It will understandably be lower than the result achieved with known future weather data, but it will be honest.

## Verdict: Build Fast and Don’t Maintain

BQML wins handily for quick prototyping. While a bespoke Python or PyTorch pipeline might eventually perform better, BigQuery provides a useful managed starting point in a fraction of the time.

Best of all, we completely bypassed the engineering lift: no infrastructure management, no API endpoints to secure, and no custom serialization or deserialization code to maintain. You get to spend your time solving the data problem, not managing the serving stack.
