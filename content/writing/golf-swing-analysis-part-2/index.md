---
title: "Golf Swing Analysis Pipeline — Part 2: LSTM Milestone Detection"
date: 2026-07-27T00:00:00-07:00
description: "Using a bidirectional LSTM and explicit motion features to locate eight ordered golf-swing milestones."
summary: "Using a bidirectional LSTM and explicit motion features to locate eight ordered golf-swing milestones."
tags: ["machine-learning", "computer-vision", "lstm", "golf"]
series: ["golf-swing-analysis"]
draft: true
showToc: true
TocOpen: false
---
# Golf Swing Analysis Pipeline — Part 2: LSTM Milestone Detection

*Working title*

Once the pipeline could recognize a golf video, the difficult part began: locating the eight frames that describe the swing, from address through finish.

A single frame can be ambiguous. A golfer’s body can look similar during the backswing and downswing even though those moments have different meanings. The surrounding frames provide the missing context.

## Treating the swing as a sequence

I used a bidirectional long short-term memory network, or BiLSTM. It processes the clip forward and backward, allowing the model to use both earlier and later frames when assigning the current label. That is possible because this is offline analysis rather than a live camera feed.

The model assigns every frame a probability for nine classes: eight milestone classes and one transition class. The milestones are Address, Toe-Up, Mid-Backswing, Top of Backswing, Mid-Downswing, Impact, Mid-Follow-Through, and Finish.

> **Figure placeholder 1 — The eight swing milestones.** Use a horizontal strip of eight frames from one analyzed swing, labeled from Address to Finish. Caption: “The model reduces a full swing to eight ordered reference frames.”

An early multiclass XGBoost baseline produced a mean absolute error of 8.41 frames. The first BiLSTM reduced it to 4.65, but two problems remained.

The model could assign two milestones to the same frame or place Impact before Mid-Downswing. I added a dynamic-programming step that finds eight distinct, high-probability frames while enforcing their physical order. This converts independent guesses into a valid sequence.

## Static coordinates were not enough

The largest improvement came from changing the inputs. The original model received 66 normalized body coordinates per frame. Although an LSTM can infer motion from changing positions, explicit motion signals worked better.

I ran a controlled ablation study across seven feature sets. Every candidate used the same 1,399-video dataset split: 1,119 training videos, 140 validation videos, and 140 held-out test videos. The comparison kept the model and evaluation procedure fixed while changing only the inputs. Aligned MAE is the average distance between predicted and labeled frames after enforcing chronology; lower is better.

| Experiment | Added information | Aligned MAE |
| --- | --- | ---: |
| A | Coordinates only | 4.07 frames |
| B | Wrist velocities | 3.66 frames |
| C | Upper-body velocities | 2.84 frames |
| D | Velocities for 12 key landmarks | 2.77 frames |
| E | All velocities plus motion summaries | **2.66 frames** |
| F | Upper-body subset plus summaries | 3.06 frames |
| G | Wrist subset plus summaries | 5.75 frames |

Experiment E combined 66 coordinates, 36 velocity features, and six summaries of motion across the wrists, arms, hips, lower body, and whole body. Its 108 inputs reduced aligned error by 34.5% relative to the coordinate-only baseline. Its 2.66-frame MAE is roughly 0.09 seconds at 30 frames per second.

> **Figure placeholder 2 — Feature ablation results.** Create a horizontal bar chart of aligned MAE for Experiments A–G, with lower values shown as better and Experiment E highlighted. Caption: “Explicit full-body motion features outperformed both static coordinates and smaller velocity subsets.”

The result was not simply “more features are better.” Wrist-only features plus summaries performed worse than the baseline. Lower-body motion contributed information about weight transfer and swing boundaries, while aggregate measures helped smooth individual landmark noise.

## Where the model is accurate—and where it is not

The winning model located Impact with an MAE of 0.63 frames and Mid-Downswing with an MAE of 0.89 frames. Across all eight milestones, 62.1% of predictions were within one frame and 80.8% were within three frames.

Address and Finish remained much harder, at 6.88 and 5.90 frames respectively. That makes sense: Impact is a sharp, high-motion event, while the beginning and end of a swing contain several nearly stationary frames that could all be reasonable labels.

> **Figure placeholder 3 — Error by milestone.** Compare coordinate-only and winning-model MAE across the eight milestones. Caption: “Motion features improved seven of eight milestones, while stationary boundary events remained the largest source of error.”

These results come from a held-out GolfDB split, not every possible filming condition. MediaPipe errors, occlusion, unusual tempos, and ambiguous annotations still propagate into the model. The conclusion is narrower: in this evaluation, representing motion explicitly was substantially more effective than asking the LSTM to infer it from positions alone.

---

**Explore the project:** [GitHub repository](https://github.com/Ameenota/golf-analysis) · [Live Streamlit app](https://golf-pro.streamlit.app/)
