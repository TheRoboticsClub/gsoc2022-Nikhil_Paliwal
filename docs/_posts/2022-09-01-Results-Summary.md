---
title: "Results summary"
excerpt: "Results!!"
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
pinned: true
---



<!-- ## TensorFlow framework -->

### Simulation

#### SimpleCircuit

All the simulations are conducted on a 4 GB `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU and batch size of 1 on SimpleCircuit. Only models which have completed one lap are presented here.

Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s) | Real time factor | Framework
--- | --- | --- | --- | --- | --- | ---
PilotNet (original) | 8.386 | 7.406 | 5.585 | 0.124 | 0.557 | TensorFlow
Dynamic Range Q | 8.534 | 6.693 | 58.474 | 0.010 | 0.54 | TensorFlow Lite
TF-TRT Baseline | 8.536 | 5.06 | **73.37** | 0.0063 | 0.47  | TensorFlow
TF-TRT FP32 | 8.32 | 4.94 | 60.28 | 0.0065 | 0.50 | TensorFlow
TF-TRT FP16 | 8.14 | 5.39 | 71.90 | 0.0056 | 0.48 | TensorFlow
TF-TRT Int8 | 8.01 | 6.65 | 59.36 | 0.0067 | 0.51  | TensorFlow
Global Prune | 7.73 | 17.03 | 42.90 | **0.0023** | 0.508 | PyTorch
Local Prune | 7.74 | 14.15 | 32.57 | 0.0027 | 0.428 | PyTorch
QAT | 7.94 | 11.47 | 26.45 | 0.0188 | 0.45 | PyTorch
Prune + Quantization | **8.84** | **4.61** | 34.40 | 0.0125 | 0.51 | PyTorch  

#### Montreal
All the simulations are conducted on a 4 GB `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU and batch size of 1.

Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s)
--- | --- | --- | --- | --- 
PilotNet (circuit not completed) | 9.431 | - | - | 0.1228
TF-TRT FP32 | 10.12 | 6.55 | 70.88 | 0.0062 
TF-TRT FP16 | 10.05 | 6.2 | 99.30 | 0.0049 
TF-TRT Int8 | 10.23 | 6.02 | 66.31 | 0.0065  


### Offline Evaluation

All evaluation on testset are conducted on a Nvidia GPU. All subsets of new datasets are used for experiment, testset - SimpleCircuit, Montreal and Montemelo circuits. 

#### Optimization strategies

Method  | Model size (MB) | MSE  | Inference time (s) | Framework
--- | --- | --- | --- | ---
PilotNet | 195 | 0.041 | 0.0364 | TensorFlow
Baseline | 64.9173469543457 | 0.04108056542969754 | 0.007913553237915039 | TensorFlow Lite
Dynamic Range Q | 16.242530822753906 | 0.04098070281274293 | 0.004902467966079712 | TensorFlow Lite
Float16 Q | 32.464256286621094 | 0.041072421023905605 | 0.007940708875656129 | TensorFlow Lite
Q aware training | 16.250564575195312 | 0.042138221871067326 | 0.009550530910491944 | TensorFlow Lite
Weight pruning | 64.9173469543457 | 0.04257505210072217 | 0.0077278904914855956 | TensorFlow Lite
Weight pruning + Q | 16.242530822753906 | 0.042606822364652304 | 0.004810283422470093 | TensorFlow Lite
Integer only Q | 16.244918823242188 | 28157.721509850544 | 0.007908073902130127 | TensorFlow Lite
Integer (float fallback) Q | 16.244888305664062 | 0.04507085706016211 | 0.00781548523902893 | TensorFlow Lite
CQAT | 16.250564575195312 | 0.0393811650675438 | 0.007680371761322021 | TensorFlow Lite
PQAT | 16.250564575195312 | 0.043669467093106665 | 0.007949142932891846 | TensorFlow Lite
PCQAT | 16.250564575195312 | **0.039242053481006144** | 0.007946955680847167 | TensorFlow Lite
Precision fp32 | 260 | 0.04103255125749467 | 0.0013057808876037597 | TensorFlow 
Precision fp16 | 260 | 0.04103255125749467 | 0.0021804444789886475 | TensorFlow 
Precision int8 | 260 | 0.04103255125749467 | **0.0011799652576446533** | TensorFlow 
Baseline | 6.118725776672363 | 0.07883577048224175 | 0.002177743434906006 | PyTorch
Dynamic Range Q | 1.9464960098266602 | 0.07840978354769981 | 0.003166124105453491 | PyTorch 
Static Q | **1.6051549911499023** | 0.07881803711366263 | 0.0026564240455627442 | PyTorch
Q aware training | 1.606736183166504 | 0.07080468241058822 | 0.0027930240631103514 | PyTorch
Local Prune | 6.119879722595215 | 0.07294941230377715 | 0.0020925970077514647 | PyTorch
Global Prune | 6.119948387145996 | 0.07079961896774226 | 0.00215102481842041 | PyTorch
Prune + Quantization | 1.606797218322754 | 0.06724451272748411 | 0.002662529468536377 | PyTorch


### Summarizing the improvements

#### TensorFlow framework
* We achieved a **~12x** reduction in the model memory size with `Dynamic range quantization`.
* We maintain a similar `MSE` value (at best 0.001 better) as baseline in offline evaluation.
* We achieved a **~33x** better inference time with `TensorRT Int8` optimization  and **~7.5x** better inference time with `Dynamic range quantization` in offline evaluation.
* We achieved **~0.66x** times smaller `Position deviation MAE` and **~12x** time higher Brain iteration frequency (RT) in simulation.
* We achieved **~22x** time improvement in `Mean inference time` in simulation.

#### PyTorch framework
* We achieved, at best, a **~4x** memory reduction with `Static Quantization technique`.
* The `MSE` is improved by 0.011 from baseline using `Prune + Quantization`. Other techniques also 
gives a slight better performance.
* All the methods inference time in same scale ( 10 <sup>-3</sup> ) of magnitude.
`Local prune` strategy has the best inference speed (cpu). 
* `Quantization + Prune` gives the best performance overall. However, all other
strategies are also very close.

### Recommendations
* PyTorch optimized models are smaller in size and have better inference time. I would recommend `Global/Local Prune` strategy for start because they also support inference with GPU. `Quantization + Prune` has least `MSE` but only support CPU inference.
* Tflite optimized models gives better performance than original models with very less memory size. The installation is easy and there is no specific hardware constraints. I would recommend `Dynamic range quantization` as first optimization method.
* TensorRT optimized models have best performance in both offline and simulation. However, they have large memory footprint. If the disk space is not a constraint, I would recommend using `Int8` or `Float16` precision model.

<!-- 
## PyTorch Framework

### Offline evaluation -->


## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://www.tensorflow.org/lite/performance/model_optimization](https://www.tensorflow.org/lite/performance/model_optimization) \\
[5] [https://www.tensorflow.org/model_optimization/guide/install](https://www.tensorflow.org/model_optimization/guide/install) \\
[6] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[7] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) \\
[8] [https://www.tensorflow.org/model_optimization/guide](https://www.tensorflow.org/model_optimization/guide)
[9] [https://pytorch.org/docs/stable/jit.html](https://pytorch.org/docs/stable/jit.html)
[10] [https://pytorch.org/blog/quantization-in-practice/](https://pytorch.org/blog/quantization-in-practice/) \\
[11] [https://pytorch.org/docs/1.12/quantization.html](https://pytorch.org/docs/1.12/quantization.html) \\
[12] [https://pytorch.org/tutorials/intermediate/pruning_tutorial.html#](https://pytorch.org/tutorials/intermediate/pruning_tutorial.html#)

