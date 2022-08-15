---
title: "Results summary"
# excerpt: "TF-TRT model verification, aggregating results, development tips"
usemathjax: true
sidebar:
  nav: "docs"

toc: true
toc_label: "Contents"
toc_icon: "cog"


categories:
- GSoC
tags:
- TensorRT
- Tensorflow
- Aggregated results
- DL Studio
- Behavior Metrics

author: Nikhil Paliwal
pinned: false
---



## TensorFlow framework

### Simulation

#### SimpleCircuit
All the simulations are conducted on a 4 GB `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU and batch size of 1 on SimpleCircuit. Only models which have completed one lap are presented here.

Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s) | Real time factor
--- | --- | --- | --- | --- | ---
PilotNet (original) | 8.386 | 7.406 | 5.585 | 0.124 | 0.557
Dynamic Range Q | 8.534 | 6.693 | 58.474 | 0.010 | 0.54
TF-TRT Baseline | **8.536** | 5.06 | **73.37** | 0.0063 | 0.47
TF-TRT FP32 | 8.32 | **4.94** | 60.28 | 0.0065 | 0.50
TF-TRT FP16 | 8.14 | 5.39 | 71.90 | **0.0056** | 0.48
TF-TRT Int8 | 8.01 | 6.65 | 59.36 | 0.0067 | 0.51 

#### Montreal
All the simulations are conducted on a 4 GB `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU and batch size of 1.

Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s)
--- | --- | --- | --- | --- 
PilotNet (circuit not completed) | 9.431 | - | - | 0.1228
TF-TRT FP32 | 10.12 | 6.55 | 70.88 | 0.0062 
TF-TRT FP16 | 10.05 | 6.2 | 99.30 | 0.0049 
TF-TRT Int8 | 10.23 | 6.02 | 66.31 | 0.0065  


### Offline Evaluation

All the simulations are conducted on a Nvidia V100 GPU with 32GB memory. The batch size was 1024. All subsets of new datasets are used for experiment, testset - SimpleCircuit, Montreal and Montemelo circuits. 

#### TFlite strategies

Method  | Model size (MB) | MSE  | Inference time (s)
--- | --- | --- | --- 
PilotNet (original tf format) | 195 | 0.041 | 0.0364
Baseline (tflite format)| 64.9173469543457 | 0.04108056542969754 | 0.007913553237915039 
Dynamic Range Q | **16.242530822753906** | 0.04098070281274293 | 0.004902467966079712
Float16 Q | 32.464256286621094 | 0.041072421023905605 | 0.007940708875656129
Weight pruning | 64.9173469543457 | 0.04257505210072217 | 0.0077278904914855956
Weight pruning + Q | **16.242530822753906** | 0.042606822364652304 | 0.004810283422470093
Integer only Q | 16.244918823242188 | 28157.721509850544 | 0.007908073902130127
Integer (float fallback) Q | 16.244888305664062 | 0.04507085706016211 | 0.00781548523902893


Method | Model size (MB) | MSE | Inference time (s)
--- | --- | --- | --- 
PilotNet (original tf format) | 195 | 0.041 | 0.0364
Baseline (tflite format)| 64.9173469543457 | 0.04108056542969754 | 0.007913553237915039 
CQAT | 16.250564575195312 | 0.0393811650675438 | 0.007680371761322021
PQAT | 16.250564575195312 | 0.043669467093106665 | 0.007949142932891846
PCQAT | 16.250564575195312 | **0.039242053481006144** | 0.007946955680847167


Method | Model size (MB) | MSE | Inference time (s)
--- | --- | --- | ---
Baseline | 64.9173469543457 | 0.04108056542590139 | 0.01011916732788086
Q aware training | 16.250564575195312 | 0.042138221871067326 | 0.009550530910491944

This can not be directly compared to other results because the performance depends a lot on hardware load and specifications, but we can compare to baseline. There is a slight improvement.

#### TensorRT (TF-TRT)

Here, I used the server with a 8 GB `NVIDIA GeForce GTX 1080/PCIe/SSE2` GPU and batch size of 128 (for inference).

Method | Model size (MB) | MSE | Inference time (s)
--- | --- | --- | --- 
Baseline | 195 | 0.041032556329194385 | 0.0012623071670532227
Precision fp32 | 260 | 0.04103255125749467 | 0.0013057808876037597
Precision fp16 | 260 | 0.04103255125749467 | 0.0021804444789886475
Precision int8 | 260 | 0.04103255125749467 | **0.0011799652576446533**

### Summarizing the improvements
* We achieved a **~12x** reduction in the model memory size with `Dynamic range quantization`.
* We maintain a similar `MSE` value (at best 0.001 better) as baseline in offline evaluation.
* We achieved a **~33x** better inference time with `TensorRT Int8` optimization  and **~7.5x** better inference time with `Dynamic range quantization` in offline evaluation.
* We achieved **~0.66x** times smaller `Position deviation MAE` and **~12x** time higher Brain iteration frequency (RT) in simulation.
* We achieved **~22x** time improvement in `Mean inference time` in simulation.


### Recommendations
* Tflite optimized models gives better performance than original models with very less memory size. The installation is easy and there is no specific hardware constraints. I would recommend `Dynamic range quantization` as first optimization method.
* TensorRT optimized models have best performance in both offline and simulation. However, they have large memory footprint. If the disk space is not a constraint, I would recommend using `Int8` or `Float16` precision model.


## PyTorch Framework

### Offline evaluation

Method | Model size (MB) | MSE | Inference time (s)
--- | --- | --- | ---
Baseline | 6.118725776672363 | 0.08102315874947678 | 0.0016005966663360596
Dynamic Range Q | 1.9464960098266602 | 0.0807250236781935 | 0.0021979384422302246
Static Q | **1.6051549911499023** | 0.08005780008222375 | 0.0020941667556762696
QAT | 1.6069040298461914 | **0.06957647389277727** | 0.0020713112354278566



## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://www.tensorflow.org/lite/performance/model_optimization](https://www.tensorflow.org/lite/performance/model_optimization) \\
[5] [https://www.tensorflow.org/model_optimization/guide/install](https://www.tensorflow.org/model_optimization/guide/install) \\
[6] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[7] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) \\
[8] [https://www.tensorflow.org/model_optimization/guide](https://www.tensorflow.org/model_optimization/guide)
