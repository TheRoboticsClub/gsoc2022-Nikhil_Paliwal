---
title: "Coding Period: Week 9"
excerpt: "Complete Pytorch quantization optimizations, montreal benchmarking and offline results"
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

Last week, I had done good good research on the existing quantization strategies supported by PyTorch. 
In result of the research and development experiments, I have implemented three varient of quantization for
PilotNet model. Moreover, I also need to change the implementation of PilotNet in PyTorch to support some
quantization operations. I benchmarked the previous trained model and retrained it with new implementation.
The retrained models has been optimized with the implemented quantization techniques and results are presented
in this blog. I have also done benchmarking of TF-TRT optimized PilotNet (TensorFlow) on Montreal circuit after
solving the issue with stat recording.

## Objectives

- [X] Benchmark the current PilotNet trained model 
- [X] Implement Static quantization
- [X] Implement Quantization aware training
- [X] Optimize the PilotNet model and do offline evaluation
- [X] TF-TRT Simulation on additional circuits

### Additional
- [X] Reimplementation and retraining of original PilotNet model in PyTorch

## Related Issues and Pull requests.

Code repository:

* torch_optim - [https://github.com/nik1806/DeepLearningStudio/tree/torch_optim](https://github.com/nik1806/DeepLearningStudio/tree/torch_optim)


## Execution

### PilotNet benchmarking and retraining

The avaible model has `MSE = 35.15` for the test circuits (SimpleCircuit, Montreal and Montemelo). 
I have done a reimplementation and retraining of the model. After which the performance improvement to
`MSE=0.086`, which is close to it's TensorFlow variant. The updated scripts of part of the new branch
`torch_optim` and I will open an PR after sufficient evaluation of the optimized models.

### Optimization and results

In total three strategies are implemented, namely, `Dynamic range quantization`, 
`Static quantization` and `Quantization aware trainig`. More theoretical details can
be found in the [last week blog](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-8/).

#### Offline evaluation
All the simulations are conducted on a ` ` GPU with ` ` GB memory. The batch size was ` `. All subsets of new datasets are used for experiment, testset - SimpleCircuit, Montreal and Montemelo circuits.

#### Observations
* baseline
* memory
* MSE
* inference time
* best performing opt strategy

### Simulation with Montreal
All the simulations are conducted on a 4 GB `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU and batch size of 1.

Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s)
--- | --- | --- | --- | --- 
TF-TRT FP32 | 10.12 | 6.55 | 70.88 | 0.0062 
TF-TRT FP16 | 10.05 | 6.2 | 99.30 | 0.0049 
TF-TRT Int8 | 10.23 | 6.02 | 66.31 | 0.0065  

## Important links

* Results page - [https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/)
* Trained weights - [https://drive.google.com/drive/folders/1j2nnmfvRdQF5Ypfv1p3QF2p2dpNbXzkt?usp=sharing](https://drive.google.com/drive/folders/1j2nnmfvRdQF5Ypfv1p3QF2p2dpNbXzkt?usp=sharing)

## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://pytorch.org/blog/quantization-in-practice/](https://pytorch.org/blog/quantization-in-practice/) \\
[5] [https://pytorch.org/docs/1.12/quantization.html](https://pytorch.org/docs/1.12/quantization.html) \\
[6] [https://pytorch.org/docs/stable/jit.html](https://pytorch.org/docs/stable/jit.html)