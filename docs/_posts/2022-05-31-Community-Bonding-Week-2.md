---
title: "Community Bonding: Week 2"
excerpt: "Identify, resolve issues and add features in main tools"
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

Through the discussion, we identify potential issues which need to be worked on before the start of the coding period. We also took a diver dive into the working of Dockers to continuously support the Behavior Metric tool. While I explore the [Quick Start](https://jderobot.github.io/BehaviorMetrics/quick_start/) section, I identify interesting scripts `show_pilots.py` and other features to visualize the performance and save the stats of the deep learning model. Furthermore, the mentors introduced me to the development of [new dataset](https://github.com/JdeRobot/DeepLearningStudio/tree/main/Formula1-FollowLine) on DeepLearningStudio [2]. After I mention an improvement in the training script, we agreed to include a validation set. In the coming weeks, we would also like to benchmark the existing models on my local machine to set a baseline for optimized models.

## Objectives

- [X] Resolve dependency issue on DeepLearningStudio's virtual environment installation
- [ ] Fix Dockerfiles to build the workflow in Behavior Metric
- [ ] Explore the `show_pilots.py` script and report the relevance of it for the project
- [ ] Update the `PilotNet` (PyTorch) model to use new dataset in DeepLearningStudio [2]
- [ ] Implement evaluation on the validation set in training scripts of models

## Issues and Pull requests.
* Created issue [Dependency conflict while installation #45](https://github.com/JdeRobot/DeepLearningStudio/issues/45) in DeepLearningStudio repo.
* Solved [issue #45](https://github.com/JdeRobot/DeepLearningStudio/issues/45) with PR [updated package versions for python3.10 #46](https://github.com/JdeRobot/DeepLearningStudio/pull/46) in DeepLearningStudio repo.

## The execution

I will include my work here.

## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio)