---
title: "Coding Period: Week 12"
excerpt: "Resolving issues, documentations, result videos"
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

This is the last week in the coding period. I will be solving the remaining issues.
Furthermore, I will document my work in a summary blog. The result blog will also be
updated. Finally, I am preparing a final video for performance comparison across 
frameworks and optimization strategies. 


## Objectives

- [X] Solve issue with Pytorch Quantization model's simulation
- [X] Record videos for simulation with quantized models.
- [X] Update `Result Summary` blog
- [X] Prepare final video for project
- [X] Document the project work in a summary blog
- [X] Update PR's

## Related Issues and Pull requests.

Related to use BehaviorMetrics repository:
* Issue: [Issue using Pytorch quantized models #396](https://github.com/JdeRobot/BehaviorMetrics/issues/396)
* PR: [PyTorch optimized model inference #399](https://github.com/JdeRobot/BehaviorMetrics/pull/399)

Related to use DeepLearningStudio repository:
* PR: [Implemented Pytorch optimization strategies #72](https://github.com/JdeRobot/DeepLearningStudio/pull/72)

## Execution

### Simulation with SimpleCircuit

The problem with using quantized models arise because Behavior Metric docker was using a older version of Pytorch (v1.08).
When I installed a more recent version of PyTorch via command 
```
pip3 install pytorch==1.11.0
```
The inference work as expected without any code change.

All the simulations are conducted on a 4 GB `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU and batch size of 1. 
I presented here the complete result table including experiments from previous week for better comparison.
For `Prune + Quantization` strategies, `Local Prune` and `Quantization aware training` are applied in succession. 

Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s)
--- | --- | --- | --- | --- 
Global Prune | 7.73 | 17.03 | 42.90 | **0.0023**
Local Prune | 7.74 | 14.15 | 32.57 | 0.0027
QAT | 7.94 | 11.47 | 26.45 | 0.0188
Prune + Quantization | **8.84** | **4.61** | 34.40 | 0.0125

* `Local Prune` gave the best performing inference time.
* Quantization strategies doesn't support inference with GPU.
* Although it was mentioned that optimized models do not support GPU inference. I found that using GPU gives a boost
in performance, e.g., inference time improved for Global pruning from 0.0071 to 0.0023 sec/frame.
* The models who were not able to complete the circuit are not mentioned above.


### Important links

* Project summary - [https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Project-Summary/](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Project-Summary/) 
* Results page - [https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/)
* Trained weights - [https://drive.google.com/drive/folders/1qTQ8Fc7OqBElU8M2llO_P-8FaA7yhiP5?usp=sharing](https://drive.google.com/drive/folders/1qTQ8Fc7OqBElU8M2llO_P-8FaA7yhiP5?usp=sharing)
* Simulation videos - [https://drive.google.com/drive/folders/1tRxVsPc0IGK2gm0gXYwy0R_mgEnHWzPp?usp=sharing](https://drive.google.com/drive/folders/1tRxVsPc0IGK2gm0gXYwy0R_mgEnHWzPp?usp=sharing)


## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://pytorch.org/tutorials/intermediate/pruning_tutorial.html#](https://pytorch.org/tutorials/intermediate/pruning_tutorial.html#)
[5] [https://pytorch.org/docs/stable/jit.html](https://pytorch.org/docs/stable/jit.html)