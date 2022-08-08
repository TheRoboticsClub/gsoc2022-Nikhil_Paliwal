---
title: "Coding Period: Week 8"
excerpt: "Pytorch optimization strategies, montreal benchmarking and additional page"
usemathjax: true
sidebar:
  nav: "docs"

toc: true
toc_label: "Contents"
toc_icon: "cog"


categories:
- GSoC
tags:
- PyTorch
- DL Studio
- Behavior Metrics

author: Nikhil Paliwal
pinned: false
---


## Preliminaries

We have explored a variety of optimization strategies available with Tensorflow framework. In last week, I aggregated all the results and also did benchmarking via simulation. For the rest of the coding period, we decide to move forward with PyTorch optimization strategies. It would provide comprehensive optimization methods to all DL users. Additionally, I tried to benchmark previous TF-TRT models on other circuits such as Montreal. And finally I put all the results in a new single page and will keep updating the new results there.

## Objectives

- [X] Research on optimization strategies in PyTorch
- [X] Prepare pytorch script for benchmarking optimized models
- [X] Implement Dynamic range quantization
- [] TF-TRT Simulation on additional circuits
- [X] Create additional page for all results 

## Related Issues and Pull requests.

Related to use BehaviorMetrics repository:
* [Crash while recording stats with PilotNet (TF) model on Montreal circuit #392](https://github.com/JdeRobot/BehaviorMetrics/issues/392)

Code repository:
* torch_optim - [https://github.com/nik1806/DeepLearningStudio/tree/torch_optim](https://github.com/nik1806/DeepLearningStudio/tree/torch_optim)

## Important links
* Results page - [https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/)
* BehaviorMetrics simulations - [https://drive.google.com/drive/folders/1ovjuWjSy-ea7YtgnaSsgVsHnbo0HJY1A?usp=sharing](https://drive.google.com/drive/folders/1ovjuWjSy-ea7YtgnaSsgVsHnbo0HJY1A?usp=sharing)
* Trained weights - [https://drive.google.com/drive/folders/1j2nnmfvRdQF5Ypfv1p3QF2p2dpNbXzkt?usp=sharing](https://drive.google.com/drive/folders/1j2nnmfvRdQF5Ypfv1p3QF2p2dpNbXzkt?usp=sharing)

## Execution

### Available strategies and guide

There is an amazing blog by PyTorch developers educating on quantization strategies [4](https://pytorch.org/blog/quantization-in-practice/). They have also provide a workflow recommendation as shown below:

![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/quantization_flowchart2_torch.png)

I have implemented basic benchmarking, dataset setup and dynamic range quantization technique. The code is present in the repository mentioned above.

### Simulation with Montreal

There is an error, which I encountered during recording stats. I have created an Issue #392, which needs to be solved and I will target next.



## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://pytorch.org/blog/quantization-in-practice/](https://pytorch.org/blog/quantization-in-practice/)