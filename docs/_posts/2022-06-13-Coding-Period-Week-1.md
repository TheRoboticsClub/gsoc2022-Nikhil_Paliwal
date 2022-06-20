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
- [X] Test access to GPU server.
- [ ] PilotNet baseline - Calculate loss (regression), inference time (script and BehaviorNet) 
- [ ] DeepestLSTMTinyPilotNet baseline - Calculate loss (regression), inference time (script and BehaviorNet)
- [ ] PilotNet optimization experiment with TensorRT
- [ ] DeepestLSTMTinyPilotNet optimization experiment with TensorRT

## Issues and Pull requests.
*

## The execution
The baseline was calculate using dataset based on circuits - Simple circuit, Montmeló and Montreal. 

## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio)
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt)
