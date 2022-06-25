---
title: "Coding Period: Week 2"
excerpt: "More on baseline and optimization experiment with TensorRT"
usemathjax: true
sidebar:
  nav: "docs"

toc: true
toc_label: "Contents"
toc_icon: "cog"


categories:
- GSoC
tags:
- Baseline
- TensorRT
- Behavior Metrics
- DL Studio

author: Nikhil Paliwal
pinned: false
---


## Preliminaries

During our meetings we discussed the possible reasons for previous week error. We also discussed about methods to calculate inference time. We have the options to use `time()` or `perf_counter()` method from `time` library [5] or follow the [blog](https://towardsdatascience.com/the-correct-way-to-measure-inference-time-of-deep-neural-networks-304a54e5187f) [4]. The blog provide more information about aspects of accurately measuring, asynchronous execution, GPU power-saving modes etc. After complete re-installation of cuda, I again start working on benchmarking baseline models. More work on previous PR was done. Moreover, I prepared scripts to evaluate PilotNet and DeepestLSTMTinyPilotNet.

## Objectives

- [X] Update previous PR's according to requested changes.
- [X] PilotNet baseline - Calculate loss (regression), inference time (script and BehaviorNet) 
- [X] DeepestLSTMTinyPilotNet baseline - Calculate loss (regression), inference time (script and BehaviorNet)
- [ ] PilotNet optimization experiment with TensorRT
- [ ] DeepestLSTMTinyPilotNet optimization experiment with TensorRT

## Related Issues and Pull requests.
* https://github.com/JdeRobot/BehaviorMetrics/pull/371

## The execution
I follow the data split defined in paper [Memory based neural networks for end-to-end autonomous driving](https://arxiv.org/abs/2205.12124). The baseline was calculate using dataset (total images - 94348) based on circuits - Simple circuit, Montmeló and Montreal. 

The following table present baseline performances:

Model | Loss | MSE | Mean Absolute Error | Inference time
--- | --- | --- | --- | ---
PilotNet | 0.041 | 0.041 | 0.095 | (0.0364 / 0.035 / 0.0323)  sec 
Deepest \\ LSTM \\ TinyPilotNet | 0.025 | 0.019 | 0.051 | (0.0358 / 0.0356 / 0.0355) sec

My code for evaluating the performance of PilotNet can be found in a separate branch [baseline-exp](https://github.com/nik1806/DeepLearningStudio/tree/baseline-exp).


## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://towardsdatascience.com/the-correct-way-to-measure-inference-time-of-deep-neural-networks-304a54e5187f](https://towardsdatascience.com/the-correct-way-to-measure-inference-time-of-deep-neural-networks-304a54e5187f) \\
[5] [https://stackoverflow.com/questions/52222002/what-is-the-difference-between-time-perf-counter-and-time-process-time](https://stackoverflow.com/questions/52222002/what-is-the-difference-between-time-perf-counter-and-time-process-time)
