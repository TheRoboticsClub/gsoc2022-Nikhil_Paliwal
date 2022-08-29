---
title: "Coding Period: Week 10 & 11"
excerpt: "Pruning, simulations, PR updates and new issues"
usemathjax: true
sidebar:
  nav: "docs"

toc: true
toc_label: "Contents"
toc_icon: "cog"


categories:
- GSoC
tags:
- Pruning
- PyTorch
- DL Studio
- Behavior Metrics

author: Nikhil Paliwal
pinned: false
---


## Preliminaries

This blog summarizes the work of last two weeks together. I have made additional changes required to the old PR's.
New optimization strategies based on pruning are implemented. Additionally, offline and online evaluation are
done to benchmark the performance of PilotNet model. I have also created new PR's to include the code into the
repositories. While, implementations and experiments I encountered some problems for which I created new issues.
The results table is also updated.


## Objectives

- [X] Implement requested changes to old PR's
- [X] Implement Local Pruning
- [X] Implement Global Pruning
- [X] Implement Pruning + Quantization
- [X] Offline evaluation for Pruning strategies
- [X] Perform online benchmarking via simulation
- [X] Record videos for simulation
- [X] Create PR's for the code implementation

## Related Issues and Pull requests.

Related to use BehaviorMetrics repository:
* Issue: [AttributeError: 'Brain' object has no attribute 'suddenness_distance' #397](https://github.com/JdeRobot/BehaviorMetrics/issues/397)
* Issue: [Issue using Pytorch quantized models #396](https://github.com/JdeRobot/BehaviorMetrics/issues/396)
* PR: [PyTorch optimized model inference #399](https://github.com/JdeRobot/BehaviorMetrics/pull/399)

Related to use DeepLearningStudio repository:
* PR: [Implemented Pytorch optimization strategies #72](https://github.com/JdeRobot/DeepLearningStudio/pull/72)

## Execution

Initially, the PilotNet model was not able to complete SimpleCircuit track. I need to train the model again for 
more epochs. However, the training with the large dataset (e.g. 48K) takes a good amount of time.  

### Offline evaluation

All the simulations are conducted on a NVIDIA GeForce 1080 GPU with 8 GB memory. The batch size was 1 for inference and 1024 for evaluation. All subsets of new datasets are used for experiment, testset - SimpleCircuit, Montreal and Montemelo circuits.

Method | Model size (MB) | MSE | Inference time (s)
--- | --- | --- | ---
Baseline | 6.118725776672363 | 0.07883577048224175 | 0.002177743434906006
Dynamic Range Q | 1.9464960098266602 | 0.07840978354769981 | 0.003166124105453491
Static Q | **1.6051549911499023** | 0.07881803711366263 | 0.0026564240455627442
QAT | 1.606736183166504 | 0.07080468241058822 | 0.0027930240631103514
Local Prune | 6.119879722595215 | 0.07294941230377715 | **0.0020925970077514647**
Global Prune | 6.119948387145996 | 0.07079961896774226 | 0.00215102481842041
Prune + Quantization | 1.606797218322754 | **0.06724451272748411** | 0.002662529468536377

#### Observations

* We store the original PilotNet model in Torchscript [5] format. So, there is no
longer need to define PilotNet model architecture to use the saved model.  
* We received, at best, a 4x memory reduction with Static Quantization technique.
* The MSE is improved by 0.011 using Quantization aware training. Other techniques also 
gives a slight better performance.
* All the methods inference time in same scale ( 10 <sup>-3</sup> ) of magnitude.
Among the optimization techniques, local pruning strategy has the best
value. 
* Quantization + Prune gives the best performance overall. However, all other
strategies are also very close.

### Simulation with SimpleCircuit
COMPLETE THIS TABLE

All the simulations are conducted on a 4 GB `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU and batch size of 1.

Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s)
--- | --- | --- | --- | --- 
PilotNet (circuit not completed) | 7.26 | - | 54.37 | 0.0049
Baseline (circuit not completed) | 7.15 | - | 55.07 | 0.0019
Global Prune | 7.73 | 17.03 | 42.90 | 0.0023 
Local Prune | 7.74 | 14.15 | 32.57 | 0.0027   

* Although it was mentioned that optimized models do not support GPU inference. I found that using GPU gives a boost
in performance, e.g., inference time improved for Global pruning from 0.0071 to 0.0023 sec/frame.
* The baseline should be able to complete the circuit. However, it could not. This indicates the original model
needs more training. 
* The quantized models were not working as expected, we can the [Issue #396](https://github.com/JdeRobot/BehaviorMetrics/issues/396) for details about the problem.

### Important links

* Results page - [https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/)
* Trained weights - [https://drive.google.com/drive/folders/1qTQ8Fc7OqBElU8M2llO_P-8FaA7yhiP5?usp=sharing](https://drive.google.com/drive/folders/1qTQ8Fc7OqBElU8M2llO_P-8FaA7yhiP5?usp=sharing)
* Simulation videos - [https://drive.google.com/drive/folders/1tRxVsPc0IGK2gm0gXYwy0R_mgEnHWzPp?usp=sharing](https://drive.google.com/drive/folders/1tRxVsPc0IGK2gm0gXYwy0R_mgEnHWzPp?usp=sharing)


## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://pytorch.org/tutorials/intermediate/pruning_tutorial.html#](https://pytorch.org/tutorials/intermediate/pruning_tutorial.html#)
[5] [https://pytorch.org/docs/stable/jit.html](https://pytorch.org/docs/stable/jit.html)