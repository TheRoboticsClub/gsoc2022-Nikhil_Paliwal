---
title: "Coding Period: Week 2"
excerpt: "Optimization with Quantization techniques"
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
- TensorRT
- Behavior Metrics
- DL Studio

author: Nikhil Paliwal
pinned: false
---


## Preliminaries
In the last week meeting, we discuss about difficulty to install TensorRT because it close to hardware and requires very specific library/packages to work. The scripts I created for baseline benchmark are useful and will be added to official repository. After using docker for TensorRT, I got a issue on my local machine - `docker: Error response from daemon: could not select device driver "" with capabilities: [[gpu]]`. I will continue solving this issue. Softwares having strict hardware dependencies are infamous for their difficulty to install in one go. Furthermore, we decide to continue pursue other optimization techniques - [Quantization](https://www.tensorflow.org/lite/performance/model_optimization). 

Quantization works by reducing the precision of the numbers used to represent a model's parameters, which by default are 32-bit floating point numbers. This results in a smaller model size and faster computation. The following table provide overview of quantizations supported by TensorFlow and their effects:

![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/quan_tech_tf.png)


## Objectives

- [ ] Prepare scripts for baseline evaluation and submit a PR
- [ ] Solve TensorRT issues
- [ ] Use Post-training quantization techniques to optimize PilotNet
- [ ] Use Post-training quantization techniques to optimize DeepestLSTMTinyPilotNet
- [ ] Prepare a script to run all optimization techniques.


## Related Issues and Pull requests.


## The execution

[Tensorflow model Optimization guide](https://www.tensorflow.org/model_optimization/guide) provide a good overview of tools to optimize machine learning inference. Currently following techniques are supported:
- [Quantization](https://www.tensorflow.org/model_optimization/guide#quantization)
- [Sparsity and pruning](https://www.tensorflow.org/model_optimization/guide#sparsity_and_pruning)
- [Clustering](https://www.tensorflow.org/model_optimization/guide#clustering)
- [Collaborative optimization](https://www.tensorflow.org/model_optimization/guide#collaborative_optimizaiton)


### Installation of TensorFlow Model Optimization
The instructions are provided [5] as:
```
# Installing with the `--upgrade` flag ensures you'll get the latest version.
pip install --user --upgrade tensorflow-model-optimization
```

### [Post-training quantization](https://www.tensorflow.org/model_optimization/guide/quantization/post_training)

The following table summarizes the choices and the benefits they provide:
![ ]({{ site.url }}{{ site.baseurl }}/assets/images/blogs/post_t_quan_summ.png)



## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://www.tensorflow.org/lite/performance/model_optimization](https://www.tensorflow.org/lite/performance/model_optimization) \\
[5] [https://www.tensorflow.org/model_optimization/guide/install](https://www.tensorflow.org/model_optimization/guide/install) \\
[7] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[8] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) \\