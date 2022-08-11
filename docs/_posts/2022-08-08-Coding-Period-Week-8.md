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
- [ ] TF-TRT Simulation on additional circuits
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

There is an amazing blog by PyTorch developers educating on quantization strategies [4](https://pytorch.org/blog/quantization-in-practice/). They have provided example codes for available strategies and methods to analyze the models. Moreover, they have also provide a workflow recommendation as shown below:

![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/quantization_flowchart2_torch.png)

The official documentation on Quantization in [5](https://pytorch.org/docs/1.12/quantization.html) present addtional important aspects to be considered.

#### [Quantization Mode Support](https://pytorch.org/docs/1.12/quantization.html#quantization-mode-support)

![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/quant_mode_support.png)

#### [Backend/Hardware Support](https://pytorch.org/docs/1.12/quantization.html#backend-hardware-support)

![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/torch_hw_support.png)

#### [Operator Support](https://pytorch.org/docs/1.12/quantization.html#operator-support)

![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/torch_operator_support.png)


I have implemented basic benchmarking, dataset setup and dynamic range quantization technique. For storing and loading the models, I have used `TorchScript` [6](https://pytorch.org/docs/stable/jit.html). TorchScript is a way to create serializable and optimizable models from PyTorch code. Any TorchScript program can be saved from a Python process and loaded in a process where there is no Python dependency. The implemented code is present in the repository mentioned above.

### Simulation with Montreal

There is an error, which I encountered during recording stats. I have created an Issue #392, which needs to be solved and I will target next. In the meeting, I got information about reasons causing the issue. The solution is to change the `PerfectLap` argument in the config file. The available options are present in `BehaviorMetrics/behavior_metrics/perfect_bags/` directory.



## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://pytorch.org/blog/quantization-in-practice/](https://pytorch.org/blog/quantization-in-practice/) \\
[5] [https://pytorch.org/docs/1.12/quantization.html](https://pytorch.org/docs/1.12/quantization.html) \\
[6] [https://pytorch.org/docs/stable/jit.html](https://pytorch.org/docs/stable/jit.html)