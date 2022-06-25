---
title: "Coding Period: Week 1"
excerpt: "Set baseline and first optimization experiment"
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

During the community bonding period, I prepared the tools for use in coding period. The tools now support training, evaluation and comparison of models in various ways. I further made changes to my PR's according to the discussion with the mentors. Next, I will start the work on optimizing the models. I will start with models trained in TensorFlow library [TF model link](https://github.com/JdeRobot/DeepLearningStudio/blob/main/Formula1-FollowLine/tensorflow/README.md). This week focus is on the architectures - PilotNet and DeepestLSTMTinyPilotNet. After setting a baseline on my personal computer, I will start experiments to optimize the models with [TensorRT](https://developer.nvidia.com/tensorrt) [3]. I also received access to Nvidia 1080 GPU machine, which would be used later for experiments.


## Objectives

- [X] Update previous PR's according to requested changes.
- [X] PilotNet baseline - Calculate loss (regression), inference time (script and BehaviorNet) 
- [ ] DeepestLSTMTinyPilotNet baseline - Calculate loss (regression), inference time (script and BehaviorNet)
- [ ] PilotNet optimization experiment with TensorRT
- [ ] DeepestLSTMTinyPilotNet optimization experiment with TensorRT

## Issues and Pull requests.
* https://github.com/JdeRobot/BehaviorMetrics/pull/371

## The execution
I follow the data split defined in paper [Memory based neural networks for end-to-end autonomous driving](https://arxiv.org/abs/2205.12124). The baseline was calculate using dataset based on circuits - Simple circuit, Montmeló and Montreal. 

The following table present baseline performances:

Model | Loss | MSE | Mean Absolute Error | Inference time
--- | --- | --- | --- | ---
PilotNet | 0.041 | 0.041 | 0.095 | 0.0364 / 0.035 / 0.0323  sec 

My code for evaluating the performance of PilotNet can be found in a separate branch [baseline-exp](https://github.com/nik1806/DeepLearningStudio/tree/baseline-exp).


## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt)
