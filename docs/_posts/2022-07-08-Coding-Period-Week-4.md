---
title: "Coding Period: Week 4"
excerpt: "Verification of optimization models (offline and simulation)"
usemathjax: true
sidebar:
  nav: "docs"

toc: true
toc_label: "Contents"
toc_icon: "cog"


categories:
- GSoC
tags:
- Quantization
- Pruning
- DL Studio
- Tensorflow
- Behavior Metrics

author: Nikhil Paliwal
pinned: false
---


## Preliminaries
The previous week a present the important theoretical background, implementation, results and observations from optimizating the PilotNet model. In this week, we focus on verifying the utility of the compressed models. We will test the model on different GPU hardward in offline fashion (using usual test scripts) and simulation with Behavior Metrics tool. It includes enabling inference scripts in Behavior Metrics to utilize the optimized models. If time permits I will continue working on TensorRT and creating optimization script for DeepestLSTMTinyPilotNet architecture.  

## Objectives

- [X] Evaluation performance of optimized models on another GPU to cross verify the results (offline fashion)
- [ ] Check the performance on a test simulation 
- [ ] Create a script to utilize optimized models in Behavior Metrics
- [ ] Compare baseline and (best) optimized model in terms of `total time`, `average speed` and `inference time` to complete a lap.
- [ ] Continue progress on other optimization strategy
<!-- - [ ] Use Post-training quantization techniques to optimize DeepestLSTMTinyPilotNet -->

### Additionally completedning quantization
  <!-- - [X] Structured pruning  -->

## Related Issues and Pull requests.
<!-- * The code is requested to be add to official repository via PR #67 [Add support for baseline evaluation and model optimization](https://github.com/JdeRobot/DeepLearningStudio/pull/67) -->
* 

## The execution

### Installation of TensorFlow Model Optimization
The instructions are provided [5] as:
```
# Installing with the `--upgrade` flag ensures you'll get the latest version.
pip install --user --upgrade tensorflow-model-optimization
```

### Verification with other GPU hardware

For cross-verification, I used my personal computer with a `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU with `Intel® Core™ i7-7700HQ CPU @ 2.80GHz × 8 ` CPU, 8 GB RAM and batch size of 1 (for inference) and 64 for loss calculation.

Method  | Model size (MB) | MSE  | Inference time (s)
--- | --- | --- | --- 
PilotNet (original tf format) | 195 | 0.041 | 0.0364
Baseline | 64.9173469543457 | 0.041032551996720734 | 0.00682752537727356
Dynamic Range Q | **16.242530822753906** | **0.040933178721450386** | **0.004078796863555908**
Float16 Q | 32.464256286621094 | 0.041024427111766355 | 0.006837798833847046
Q aware training | **16.242530822753906** | **0.040933178721450386** | 0.004129417657852173
Weight pruning | 64.9173469543457 | 0.04252005337036257 | 0.006857085466384888
Weight pruning + Q | **16.242530822753906** | 0.04255170110407089 | 0.004178661823272705
Integer only Q | 16.244918823242188 | 28147.113743361497 | 0.00712398886680603
Integer (float fallback) Q | 16.244888305664062 | 0.04501058531541387 | 0.007014316082000732


*All the results are for model converted to tflite models if not specified.*

#### Conclusions
* **The improvements are consistent with previous week results - same model compression rate, no increase in MSE and even a little improvement in inference time (better by 0.0009 s).**
* The baseline / original model also got improvements in model size (195 -> 64.9 MB) and inference time (0.0364 -> 0.00791) when converted to `tflite` format without increase in mean square error (MSE).
* **Dynamic range quantization** strategy gives best model in all aspects - <br>
    model size: 4x reduction <br>
    MSE: slightly better (0.0001 reduction) <br>
    Inference time: ~1.7x reduction
* Also, the most faster and economical choice would be use **Dynamic range quantization** because it doesn't need any dataset or time to train.
* I recommend trying both `Dynamic range quantization` and `Quantization aware training` chose the best because both have competing performance.
* We also found that integer quantization is not suitable for our model, because we perform a regression task. The precision loss due to quantization severly increase our MSE, making its use impossible.
* From the original model (in tensorflow format), we achieved - <br>
    model size: 10x reduction <br>
    MSE: slightly better (0.0001 reduction) <br>
    Inference time: ~7.7x reduction

<!-- ### Experimental setup
For using the complete dataset, I need a more powerful machine. I used a Nvidia V100 GPU with 32GB memory. The batch size was 1024. All subsets of new datasets are used for experiment.   -->

All the optimized models are publicly available in the [google drive](https://drive.google.com/drive/folders/1j2nnmfvRdQF5Ypfv1p3QF2p2dpNbXzkt?usp=sharing).

## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://www.tensorflow.org/lite/performance/model_optimization](https://www.tensorflow.org/lite/performance/model_optimization) \\
[5] [https://www.tensorflow.org/model_optimization/guide/install](https://www.tensorflow.org/model_optimization/guide/install) \\
[6] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[7] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) 
