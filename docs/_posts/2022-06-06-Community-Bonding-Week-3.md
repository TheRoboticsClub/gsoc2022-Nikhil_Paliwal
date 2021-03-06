---
title: "Community Bonding: Week 3"
excerpt: "Continuing - resolving issues and adding features in main tools"
usemathjax: true
sidebar:
  nav: "docs"

toc: true
toc_label: "Contents"
toc_icon: "cog"


categories:
- GSoC
tags:
- Docker
- Virtualenv
- Behavior Metrics
- DL Studio

author: Nikhil Paliwal
pinned: false
---


## Preliminaries

I identified some errors while using the existing frameworks. This week, we will continue to fix the issues found and discussed during the meeting. I also verified the inference time for PilotNet model (PyTorch implementation), the difference between the CPU vs GPU execution is (0.0265  vs 0.0192) seconds (mean inference time) for simulation of 1 minute.

![ ]({{ site.url }}{{ site.baseurl }}/assets/images/sim_stat_metrics.png)

When we save stats of a simulation a window shows all metrics in details. We will also use this in later phases to evaluate and compare our optimized models. I describe our new objectives next.


## Objectives

- [X] Solve the issue [Error while trying to save stats with DL-torch.yml config #368](https://github.com/JdeRobot/BehaviorMetrics/issues/368) and submit a PR
- [X] Fix the issue [Not utilizing GPU when running simulation #369](https://github.com/JdeRobot/BehaviorMetrics/issues/369).
- [X] Update PR [updated package versions for python3.10 #46](https://github.com/JdeRobot/DeepLearningStudio/pull/46) to remove additional packages and include installation instructions in REAMDE
- [X] Update PR [Use new dataset #49](https://github.com/JdeRobot/DeepLearningStudio/pull/49) - Add feature to let user specify each dataset directory and normalize label values.
- [X] Update PilotNet brain in BehaviorNet to unnormalize (expand) the predicted values
- [X] Implement evaluation, on a validation set, in the training scripts of models

## Issues and Pull requests.
* Submitted PR [Removed: unnecessary exp details #370](https://github.com/JdeRobot/BehaviorMetrics/pull/370) to fix [issue #368](https://github.com/JdeRobot/BehaviorMetrics/issues/368).
* Solved [issue #369](https://github.com/JdeRobot/BehaviorMetrics/issues/369) by PR [Update: brain file to use gpu #371](https://github.com/JdeRobot/BehaviorMetrics/pull/371).
* Updated PR [updated package versions for python3.10 #46](https://github.com/JdeRobot/DeepLearningStudio/pull/46).
* Updated PR [Use new dataset #49](https://github.com/JdeRobot/DeepLearningStudio/pull/49).
* Create a PR [unnormalized prediction value #372](https://github.com/JdeRobot/BehaviorMetrics/pull/372) to unnormalize (expand) the predicted values of PilotNet brain in BehaviorNet.
* Updated PilotNet training script with PR [Adding validation set for model selection #50](https://github.com/JdeRobot/DeepLearningStudio/pull/50). 

## The execution

This week was dedicated to resolves the previously found issues. Moreover I also included new features which would be helpful in conducting experiments and evaluation. For the training part, I worked on finalizing installation packages, enable training scripts to use GPU, also update it to use new training datasets and evaluate on a validation set. Furthermore, the inference scripts on BehaviorMetrics were updated accordingly. 


## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio)
