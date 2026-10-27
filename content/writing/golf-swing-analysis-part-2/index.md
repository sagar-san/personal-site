---
title: "Golf Swing Analysis Pipeline — Part 2: LSTM Milestone Detection"
date: 2026-09-15T09:00:00-07:00
description: "Using a bidirectional LSTM and explicit motion features to locate eight ordered golf-swing milestones."
summary: "Using a bidirectional LSTM and explicit motion features to locate eight ordered golf-swing milestones."
tags: ["machine-learning", "computer-vision", "lstm", "golf"]
series: ["golf-swing-analysis"]
draft: false
showToc: true
TocOpen: false
cover:
  image: "video-poster.jpg"
  alt: "Golf swing milestone comparison alongside a Sandra Gal reference swing"
  hiddenInList: false
  hiddenInSingle: true
---
In [Part 1]({{< relref "golf-swing-analysis-part-1" >}}), I used MediaPipe to extract x and y coordinates for 33 body landmarks in each video frame, then used XGBoost to check whether the clip contained a golf swing.

The next step was to locate the frames corresponding to key swing milestones, such as address, toe-up, the top of the backswing, and impact. This article covers how I used those saved coordinates to identify eight milestones across the swing.

> **Try the live demo:** [Open the Golf Swing Analyzer](https://golf-pro.streamlit.app/) and choose a preset swing to explore the results, or upload your own video.

I first tried a multiclass XGBoost model for this task, then moved to a bidirectional LSTM. In the initial experiments, the LSTM located milestones more accurately: mean absolute error fell from 8.41 to 4.65 frames. XGBoost remained useful for the separate golf/non-golf gatekeeper described in Part 1.

A single frame can be ambiguous. A golfer’s body can look similar during the backswing and downswing even though those moments have different meanings. The surrounding frames provide the missing context.

## Treating the swing as a sequence

I used a bidirectional long short-term memory network, or BiLSTM. It processes the clip forward and backward, allowing the model to use both earlier and later frames when assigning the current label. That is possible because this is offline analysis rather than a live camera feed.

The model assigns every frame a probability for nine classes: eight milestone classes and one transition class. The milestones are Address, Toe-Up, Mid-Backswing, Top of Backswing, Mid-Downswing, Impact, Mid-Follow-Through, and Finish.

{{< youtube OAuj_g5Eek0 >}}

*Example output from the completed pipeline: a user swing aligned with a Sandra Gal reference using the detected milestones. The video illustrates the output, rather than measuring prediction accuracy.*

The lower frame error made the BiLSTM the better milestone detector in these experiments, but it did not guarantee a valid sequence of events.

The model could assign two milestones to the same frame or place Impact before Mid-Downswing. I added a dynamic-programming step that finds eight distinct, high-probability frames while enforcing their physical order. This converts independent guesses into a valid sequence.

## Static coordinates were not enough

The next experiment changed the inputs. The original model received 66 normalized body coordinates per frame. Although an LSTM can infer motion from changing positions, explicit motion signals worked better.

I ran a controlled study across seven feature sets. The experiment log reports a 1,399-video dataset split: 1,119 training videos, 140 validation videos, and 140 held-out test videos. The comparison kept the model and evaluation procedure fixed while changing only the inputs. The training notebooks split by video ID, keeping frames from each clip together; they do not enforce separation by golfer.

The [experiment log](https://github.com/sagar-san/golf-analysis/blob/main/docs/experiments.md) reports the candidate comparisons on the test split, so these are exploratory benchmark results rather than a final untouched test of the selected model.

Aligned MAE is the average distance between predicted and labeled frames after enforcing chronology; lower is better.

| Experiment | Added information                    | Aligned MAE     |
| ---------- | ------------------------------------ | --------------: |
| A          | Coordinates only                     | 4.07 frames     |
| B          | Wrist velocities                     | 3.66 frames     |
| C          | Upper-body velocities                | 2.84 frames     |
| D          | Velocities for 12 key landmarks       | 2.77 frames     |
| E          | All velocities plus motion summaries | **2.66 frames** |
| F          | Upper-body subset plus summaries     | 3.06 frames     |
| G          | Wrist subset plus summaries          | 5.75 frames     |

Experiment E combined 66 coordinates, 36 velocity features, and six summaries of motion across the wrists, arms, hips, lower body, and whole body. Its 108 inputs reduced aligned error by 34.5% relative to the coordinate-only baseline. At 30 frames per second, 2.66 frames corresponds to roughly 0.09 seconds of playback; elapsed time depends on the recording frame rate.

The result was not simply “more features are better.” Wrist-only features plus summaries performed worse than the baseline. My interpretation is that lower-body motion provides useful context for swing boundaries and aggregate measures may reduce the effect of individual landmark noise. This comparison does not isolate those mechanisms.

## Where the model is accurate—and where it is not

The winning model located Impact with an MAE of 0.63 frames and Mid-Downswing with an MAE of 0.89 frames. Across all eight milestones, 62.1% of predictions were within one frame and 80.8% were within three frames.

Address and Finish remained much harder, at 6.88 and 5.90 frames respectively. That makes sense: Impact is a sharp, high-motion event, while the beginning and end of a swing contain several nearly stationary frames that could all be reasonable labels.

These results come from a held-out GolfDB split, not every possible filming condition. MediaPipe errors, occlusion, unusual tempos, and ambiguous annotations still propagate into the model. The conclusion is narrower: in this evaluation, representing motion explicitly was substantially more effective than asking the LSTM to infer it from positions alone.

The saved [initial LSTM notebook](https://github.com/sagar-san/golf-analysis/blob/main/notebooks/lstm_keras.ipynb) ran on CPU. For this project, working with saved numerical pose features made model training practical without a powerful GPU.

Read [Part 1: MediaPipe and XGBoost]({{< relref "golf-swing-analysis-part-1" >}}) for the preprocessing and golf/non-golf filter.

---

**Explore the project:** [GitHub repository](https://github.com/sagar-san/golf-analysis) · [Live Streamlit app](https://golf-pro.streamlit.app/)
