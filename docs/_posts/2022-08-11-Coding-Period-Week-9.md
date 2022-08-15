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
`MSE=0.0719`, which is close to it's TensorFlow variant. The updated scripts of part of the new branch
`torch_optim` and I will open an PR after sufficient evaluation of the optimized models.

### Optimization and results

In total three strategies are implemented, namely, `Dynamic range quantization`, 
`Static quantization` and `Quantization aware trainig`. More theoretical details can
be found in the [last week blog](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-8/).

#### Offline evaluation
All the simulations are conducted on a NVIDIA GeForce 1080 GPU with 8 GB memory. The batch size was 1 for inference and 1024 for evaluation. All subsets of new datasets are used for experiment, testset - SimpleCircuit, Montreal and Montemelo circuits.

#### Result table

Method | Model size (MB) | MSE | Inference time (s)
--- | --- | --- | ---
Baseline | 6.118725776672363 | 0.08102315874947678 | 0.0016005966663360596
Dynamic Range Q | 1.9464960098266602 | 0.0807250236781935 | 0.0021979384422302246
Static Q | **1.6051549911499023** | 0.08005780008222375 | 0.0020941667556762696
QAT | 1.6069040298461914 | **0.06957647389277727** | 0.0020713112354278566

#### Observations

* We store the original PilotNet model in Torchscript [6] format. So, there is no
longer need to define PilotNet model architecture to use the saved model.  
* We received, at best, a 3x memory reduction with Static Quantization technique.
* The MSE is improved by 0.011 using Quantization aware training. Other techniques also 
gives a slight better performance.
* All the methods inference time in same scale ( 10 <sup>-3</sup> ) of magnitude.
Among the optimization techniques, quantization aware training strategy has the best
value. 
* Quantization aware strategy gives the best performance overall. However, all other
strategies are also very close.

### Simulation with Montreal
All the simulations are conducted on a 4 GB `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU and batch size of 1.

Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s)
--- | --- | --- | --- | --- 
PilotNet (circuit not completed) | 9.431 | - | - | 0.1228
TF-TRT FP32 | 10.12 | 6.55 | 70.88 | 0.0062 
TF-TRT FP16 | 10.05 | 6.2 | 99.30 | 0.0049 
TF-TRT Int8 | 10.23 | 6.02 | 66.31 | 0.0065  

## Important links

* Results page - [https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/)
* Trained weights - [https://drive.google.com/drive/folders/1qTQ8Fc7OqBElU8M2llO_P-8FaA7yhiP5?usp=sharing](https://drive.google.com/drive/folders/1qTQ8Fc7OqBElU8M2llO_P-8FaA7yhiP5?usp=sharing)
## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://pytorch.org/blog/quantization-in-practice/](https://pytorch.org/blog/quantization-in-practice/) \\
[5] [https://pytorch.org/docs/1.12/quantization.html](https://pytorch.org/docs/1.12/quantization.html) \\
[6] [https://pytorch.org/docs/stable/jit.html](https://pytorch.org/docs/stable/jit.html)