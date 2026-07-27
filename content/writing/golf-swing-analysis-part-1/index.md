---
title: "Golf Swing Analysis Pipeline — Part 1: MediaPipe and XGBoost"
date: 2026-07-27T00:00:00-07:00
description: "Turning golf videos into pose landmarks and using an XGBoost gatekeeper to reject unsuitable inputs."
summary: "Turning golf videos into pose landmarks and using an XGBoost gatekeeper to reject unsuitable inputs."
tags: ["machine-learning", "computer-vision", "mediapipe", "xgboost", "golf"]
series: ["golf-swing-analysis"]
draft: true
showToc: true
TocOpen: false
---
# Golf Swing Analysis Pipeline — Part 1: MediaPipe and XGBoost

*Working title*

I started this project to build a more complete video-analysis pipeline than the smaller machine-learning projects I had worked on before. The first problem was turning an ordinary phone video into consistent data and deciding whether it contained a golf swing at all. MediaPipe and XGBoost offered established tools for that first stage.

## Turning frames into body coordinates

A model cannot reason about concepts such as “the hands moved above the shoulders” until the video is converted into a more useful representation.

I used MediaPipe Pose Landmarker in video mode. For each frame, it estimates 33 body landmarks, including the shoulders, elbows, wrists, hips, knees, and ankles. The result is a time series of body coordinates rather than a stack of raw pixels. A four-second clip can now be represented as rows in a table, with one row per frame.

> **Figure placeholder 1 — From video to landmarks.** Use a three-panel image showing an original frame, the MediaPipe skeleton overlaid on the golfer, and a simplified plot of normalized landmark coordinates. Suggested caption: “MediaPipe reduces each video frame to a consistent set of body landmarks.”

Pose estimates can jitter, and a joint may disappear briefly during fast motion. I interpolate short gaps, smooth the tracks, center the body around the hips, and scale the coordinates using torso length. The model should respond to a swing pattern, not the golfer’s distance from the camera.

MediaPipe also defines an important limitation: it tracks the body, not the club or ball. The later models must infer golf events from body motion alone.

## Why add a gatekeeper?

The rest of the pipeline assumes it has received a golf swing. If someone uploads a walking clip, passing it directly to the milestone model will still produce eight predictions; they will simply be meaningless. I therefore added a binary gatekeeper before the more expensive analysis.

The gatekeeper uses XGBoost, a gradient-boosted tree model that works well with structured, tabular features. Each frame begins with 66 normalized coordinates—an x and y value for each landmark. I add 32 offset features from selected high-motion joints at nearby frames, giving the model 98 inputs that describe both pose and short-term movement.

XGBoost was a practical choice: training is quick, inference is fast, the model is compact, and its probability scores make threshold tuning straightforward. This stage needed an inexpensive early filter, not a large sequence network.

> **Figure placeholder 2 — Gatekeeper flow.** Show: uploaded video → MediaPipe landmarks → smoothing and normalization → 98 tabular features → XGBoost probability → accept or reject.

## What the evaluation showed

On the held-out frame-level test split, the classifier reached 96.4% accuracy, with 96% precision and 97% recall for golf frames. Those numbers were encouraging, but frame-level performance does not fully describe how an uploaded video behaves.

I also evaluated the gatekeeper on 305 held-out videos: 141 golf swings and 164 non-golf actions. Averaging predictions over an entire clip produced 93.1% accuracy. That approach can dilute a short swing inside a longer recording, so I instead scored the strongest two-second rolling window. At the evaluated threshold of 0.7346, accuracy rose to 95.1%. Golf precision—the share of accepted clips that were golf—was 100%. Recall—the share of real golf clips accepted—was 89.4%.

| Video-level method | Accuracy | Golf precision | Golf recall |
| --- | ---: | ---: | ---: |
| Full-video mean | 93.1% | 100.0% | 85.1% |
| Best two-second window | 95.1% | 100.0% | 89.4% |

> **Figure placeholder 3 — Video-level gatekeeper results.** Use grouped bars comparing accuracy and golf recall for full-video averaging versus the two-second rolling window. Caption: “Local scoring recovered short swings that were diluted by inactive frames.”

The precision–recall balance is a product decision. A stricter threshold avoids invalid uploads but rejects more real swings. The deployed pipeline uses a separately tuned threshold that favors accepting plausible golf footage.

This first stage is intentionally straightforward: reduce the video to body movement, normalize it, and use a fast tabular model to reject unsuitable inputs. The harder problem is deciding exactly where the swing’s eight key events occur. That is where the LSTM enters the pipeline.

---

**Explore the project:** [GitHub repository](https://github.com/Ameenota/golf-analysis) · [Live Streamlit app](https://golf-pro.streamlit.app/)
