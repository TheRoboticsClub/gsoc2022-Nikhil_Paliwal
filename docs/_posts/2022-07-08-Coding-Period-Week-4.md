---
title: "Coding Period: Week 4"
excerpt: "Verification of optimizated models (offline and simulation)"
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
The previous week, I presented the important theoretical background, implementation, results and observations from optimizating the PilotNet model. In this week, we focus on verifying the utility of the compressed models. We will test the models on different GPU hardward in offline fashion (using usual test scripts) and via simulation with Behavior Metrics tool. It includes enabling inference scripts in Behavior Metrics to utilize the optimized models. If time permits I will continue working on TensorRT and creating optimization script for DeepestLSTMTinyPilotNet architecture.  

## Objectives

- [X] Evaluation performance of optimized models on another GPU to cross verify the results (offline fashion)
- [X] Check the performance on a test simulation 
- [X] Create a script to utilize optimized models in Behavior Metrics
- [X] Benchmark baseline PilotNet modle in terms of `inference time`, `average speed` and `position deviation MAE` to complete a lap.
- [X] Dynamic range quantized PilotNet model in terms of `inference time`, `average speed` and `position deviation MAE` to complete a lap.
- [X] Quantized aware trained PilotNet model in terms of `inference time`, `average speed` and `position deviation MAE` to complete a lap.
- [X] Pruned models in terms of `inference time`, `average speed` and `position deviation MAE` to complete a lap.
- [X] Compare the simulation results and draw conclusions
- [X] Continue progress on other optimization strategy
<!-- - [ ] Use Post-training quantization techniques to optimize DeepestLSTMTinyPilotNet -->

### Additionally completed
- [X] New issues and solution for execution of TensorFlow supported brain on BehaviorMetrics 
  <!-- - [X] Structured pruning  -->

## Related Issues and Pull requests.

Related to use BehaviorMetrics repository:
* [ImportError: cannot import name 'ft2font' from 'matplotlib' #383](https://github.com/JdeRobot/BehaviorMetrics/issues/383)
* [AttributeError: 'Brain' object has no attribute 'suddenness_distance' #384](https://github.com/JdeRobot/BehaviorMetrics/issues/384)
* [Skipping registering GPU devices #385](https://github.com/JdeRobot/BehaviorMetrics/issues/385)

PR to sumbit new script:
* [Update the scripts to support optimized tflite models #386](https://github.com/JdeRobot/BehaviorMetrics/pull/386).

## The execution

The issue on my local machine mentioned in last week blog - `docker: Error response from daemon: could not select device driver "" with capabilities: [[gpu]]` was solved by following the recommendation from [nvidia forum](https://forums.developer.nvidia.com/t/could-not-select-device-driver-with-capabilities-gpu/80200). The provided server is a docker container and I expect some support from mentor for additional setup because I can not directly use TensorRT docker on another docker container.  

**All the optimized models are publicly available in the [google drive](https://drive.google.com/drive/folders/1j2nnmfvRdQF5Ypfv1p3QF2p2dpNbXzkt?usp=sharing).**

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


*All the results are for models converted to tflite models if not specified.*

#### Conclusions
* **The improvements are consistent** with previous week results - same model compression rate, no increase in MSE and even a little improvement in inference time (better by 0.0009 s).
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

### Performance on simulation (online fashion)

I used my personal computer with a `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU with `Intel® Core™ i7-7700HQ CPU @ 2.80GHz × 8 ` CPU, 8 GB RAM and batch size of 1 (for inference). The `stats` are recorded after approximately one lap and for comparison, focus should be on the aspect of average calculations. The current BehaviorMetrics tool need more updates for working with a Tensorflow environment and I have tried to provide support by creating and solving issues. Although, I lot of reinstallations and time  has been diverted in making environment, cuda, cudnn, VNC and docker work together.  

#### Baseline
![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/baseline_pilotnet.png)

#### Dynamic range quantization
![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/dynamic_q_res.png)

#### Quantization aware training
![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/q_aware_res.png)

#### Rest optmization strategies
Other strategies were not able to complete one lap properly. So, they are excluded from comparison.

#### Comparison table


Method  | Average speed | Position deviation MAE | Brain iteration frequency (RT) | Mean Inference time (s)
--- | --- | --- | --- | ---
Baseline | 8.386 | 7.406 | 5.585 | 0.124
Dynamic Range Q | 8.534 | 6.693 | 58.474 | 0.010
Q aware training | 8.472 | 5.001 | 58.09 | 0.010

#### Conclusion
* The benefits obtained from optimizing models are relevant for both offline and online (simulation) usecases.
* We observe slight improvement in `Average speed` and `Position deviation MAE`.
* On average, quantization strategy gives a boost of ~10x times to other aspects (inferece time etc.).
* The optimized model complete lap 1 second faster (in 50s) as compare to baseline (in 51s).
* Unfortunately, other strategies such as pruning and integer quantization are not appropriate to be used in simulation.

## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://www.tensorflow.org/lite/performance/model_optimization](https://www.tensorflow.org/lite/performance/model_optimization) \\
[5] [https://www.tensorflow.org/model_optimization/guide/install](https://www.tensorflow.org/model_optimization/guide/install) \\
[6] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[7] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) 
