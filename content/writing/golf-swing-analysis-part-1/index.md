---
title: "Golf Swing Analysis Pipeline — Part 1: MediaPipe and XGBoost"
date: 2026-09-01T09:00:00-07:00
description: "Turning golf videos into pose landmarks and using an XGBoost gatekeeper to reject unsuitable inputs."
summary: "Turning golf videos into pose landmarks and using an XGBoost gatekeeper to reject unsuitable inputs."
tags: ["machine-learning", "computer-vision", "mediapipe", "xgboost", "golf"]
series: ["golf-swing-analysis"]
draft: false
showToc: true
TocOpen: false
cover:
  image: "video-poster.jpg"
  alt: "Golf swing pose landmarks alongside a Greg Norman reference swing"
  hiddenInList: false
  hiddenInSingle: true
---
I built this project with AI assistance as a way to learn how to make a fast video analyzer. The first problem was turning a low video into consistent data and deciding whether it contained a golf swing at all.

> **Try the live demo:** [Open the Golf Swing Analyzer](https://golf-pro.streamlit.app/) and choose a preset swing to explore the results, or upload your own video.

## Turning frames into body coordinates

A model cannot reason about concepts such as “the hands moved above the shoulders” until the video is converted into a more useful representation (aka numbers).

I used MediaPipe Pose Landmarker in video mode. For each frame, it estimates 33 body landmarks, including the shoulders, elbows, wrists, hips, knees, and ankles. The result is a time series of body coordinates rather than a stack of raw pixels. I saved these features as CSV files so I could retrain models without running pose extraction again. That made iteration much faster. A four-second clip can now be represented as rows in a table, with one row per frame.

{{< youtube coD5eBGOvkY >}}

*Example output from the completed pipeline: pose landmarks overlaid on a user swing and a Greg Norman reference. The milestone labels and measurements come from later stages of the pipeline.*

Pose estimates can jitter, and a joint may disappear briefly during fast motion. I interpolate short gaps, smooth the tracks, center the body around the hips, and scale the coordinates using torso length. The model should respond to a swing pattern, not the golfer’s distance from the camera.

MediaPipe also defines an important limitation: it tracks the body, not the club or ball. The later models must infer golf events from body motion alone.

## Why add a gatekeeper?

The rest of the pipeline assumes it has received a golf swing. If someone uploads a walking clip, passing it directly to the milestone model will still produce eight predictions; they will simply be meaningless. I therefore added a binary gatekeeper before the more expensive analysis.

The gatekeeper uses XGBoost, a gradient-boosted tree model that works well with structured, tabular features. Each frame begins with 66 normalized coordinates—an x and y value for each landmark. I add 32 offset features from selected high-motion joints at nearby frames, giving the model 98 inputs that describe both pose and short-term movement.

XGBoost was a practical choice: training is quick, inference is fast, the model is compact, and its probability scores make threshold tuning straightforward. This stage needed an inexpensive early filter, not a large sequence network.


## What the evaluation showed

On the held-out frame-level test split, the classifier reached 96.4% accuracy, with 96% precision and 97% recall for golf frames. Those numbers were encouraging, but frame-level performance does not fully describe how an uploaded video behaves.

For video-level decisions, the pipeline averages frame probabilities over a sliding two-second window and uses the strongest window as the clip score. This avoids diluting a short swing with inactive frames elsewhere in the recording.

The [training notebook](https://github.com/sagar-san/golf-analysis/blob/main/notebooks/xgb_binary_detector.ipynb) keeps all frames from each video in the same split. It splits by video ID, so these results do not establish performance on entirely unseen golfers.

This first stage is intentionally straightforward: reduce the video to body movement, normalize it, and use a fast tabular model to reject unsuitable inputs. The harder problem is deciding exactly where the swing’s eight key events occur. That is where the LSTM enters the pipeline.

Continue with [Part 2: LSTM Milestone Detection]({{< relref "golf-swing-analysis-part-2" >}}).

---

**Explore the project:** [GitHub repository](https://github.com/sagar-san/golf-analysis) · [Live Streamlit app](https://golf-pro.streamlit.app/)
