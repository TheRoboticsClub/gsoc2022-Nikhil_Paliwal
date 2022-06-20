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
* While working on baseline I came across issue [Missing argument: learning_rate #55](https://github.com/JdeRobot/DeepLearningStudio/issues/55)

## The execution
I follow the data split defined in paper [Memory based neural networks for end-to-end autonomous driving](https://arxiv.org/abs/2205.12124). The baseline was calculate using dataset based on circuits - Simple circuit, Montmeló and Montreal. I came across a time consuming error regarding cuda `Could not load dynamic library 'libcudart.so.11.0'; dlerror: libcudart.so.11.0`. It took me quite some time to solve the issue and installing new Cuda package is also time consuming. I need to reinstall the complete GPU (cuda) environment.
 <!-- and found suitable instructions in the [blog](https://medium.com/@anarmammadli/how-to-install-cuda-11-4-on-ubuntu-18-04-or-20-04-63f3dee2099) for Ubuntu 18.04 and Cuda 11.4. -->

## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio)
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt)
