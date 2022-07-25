---
title: "Coding Period: Week 6"
excerpt: "Analyze results, failures and TensorRT (TF-TRT) Optimization"
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
- Video
- DL Studio
- Behavior Metrics

author: Nikhil Paliwal
pinned: false
---


## Preliminaries
After presenting the simulation videos, we decided to analyze the failure cases. I will also combine the best performing models in single video for presentation and comparison. Furthermore, some PR and issues are created. Most importantly, I worked on using TensorRT to optimize the tensorflow models. Using TensorRT (here [TF-TRT](https://blog.tensorflow.org/2021/01/leveraging-tensorflow-tensorrt-integration.html)) is not typical and therefore I have shared many resources in the blog to understand it. 

## Objectives

- [X] Record F1 car crashing videos and their stats
- [X] Record stats for Dynamic range quantization model on Montreal circuit
- [X] Combine the best performing simulation in one video with explainations
- [X] Updata [PR #67](https://github.com/JdeRobot/DeepLearningStudio/pull/67) with results table, inference script and weight links
- [X] Prepare TensorRT scripts for optimization (float32, float16 and int8 modes)
- [X] Evaluate TensorRT in offline mode


## Related Issues and Pull requests.

Related to use BehaviorMetrics repository:
* While recording the stats for simulation, I encountered a issue - [Crash while recording stats with PilotNet (TF) model on Montreal circuit #392](https://github.com/JdeRobot/BehaviorMetrics/issues/392). 

Updated PR:
* [Add support for baseline evaluation and model optimization #67](https://github.com/JdeRobot/DeepLearningStudio/pull/67)

Repository for new script:
* [https://github.com/nik1806/DeepLearningStudio/tree/tf_trt](https://github.com/nik1806/DeepLearningStudio/tree/tf_trt)

## The execution

### Video demonstration
 I used my personal computer with a `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU with `Intel® Core™ i7-7700HQ CPU @ 2.80GHz × 8 ` CPU, 8 GB RAM and batch size of 1 for simulation.

The crash videos are avaiable on google drive - [https://drive.google.com/drive/folders/1ovjuWjSy-ea7YtgnaSsgVsHnbo0HJY1A?usp=sharing](https://drive.google.com/drive/folders/1ovjuWjSy-ea7YtgnaSsgVsHnbo0HJY1A?usp=sharing)

#### Final video
<iframe width="560" height="315" src="https://www.youtube.com/embed/OcEoM9h2-5Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Optimization with TensorRT

I used the server with a * GB `NVIDIA GeForce GTX 1080/PCIe/SSE2` GPU and batch size of 1 (for inference).

There are some useful resources, I have listed below:
* User Guide - [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html)
* TF-TRT blog - [https://blog.tensorflow.org/2021/01/leveraging-tensorflow-tensorrt-integration.html](https://blog.tensorflow.org/2021/01/leveraging-tensorflow-tensorrt-integration.html)
* Inference example - [github_repo](https://github.com/tensorflow/tensorrt/blob/1a90ce302a9397331eb8b5624cf9410c96a4dd29/tftrt/blog_posts/Leveraging%20TensorFlow-TensorRT%20integration%20for%20Low%20latency%20Inference/tf2_inference.py)
* TensorRT API - [https://www.tensorflow.org/versions/r2.4/api_docs/python/tf/experimental/tensorrt](https://www.tensorflow.org/versions/r2.4/api_docs/python/tf/experimental/tensorrt)
* Dynamic image size example - [notebook_link](https://notebooks.githubusercontent.com/view/ipynb?browser=chrome&color_mode=auto&commit=65ed25ccd5d538d6a42223cb69c2fb9858f39805&device=unknown&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f74656e736f72666c6f772f74656e736f7272742f363565643235636364356435333864366134323232336362363963326662393835386633393830352f74667472742f6578616d706c65732f70726573656e746174696f6e732f4754432d417072696c323032312d44796e616d69632d73686170652d5265734e657456322e6970796e62&logged_in=false&nwo=tensorflow%2Ftensorrt&path=tftrt%2Fexamples%2Fpresentations%2FGTC-April2021-Dynamic-shape-ResNetV2.ipynb&platform=android&repository_id=157606202&repository_type=Repository&version=98)
* Basic colab example - [notebook_link](https://colab.research.google.com/github/vinhngx/tensorrt/blob/vinhn-tf20-notebook/tftrt/examples/image-classification/TFv2-TF-TRT-inference-from-Keras-saved-model.ipynb#scrollTo=01lqtLJzvwSS)

The scripts are available here - [https://github.com/nik1806/DeepLearningStudio/tree/tf_trt](https://github.com/nik1806/DeepLearningStudio/tree/tf_trt)

#### Result table
I am using tensorflow graphs (low-level API) for inference here. 

Method | Model size (MB) | MSE | Inference time (s)
--- | --- | --- | --- 
Baseline | 196 | 0.041032556329194385 | 0.0012623071670532227
Precision fp32 | 260 | 0.04103255125749467 | 0.0013057808876037597
Precision fp16 | 260 | 0.04103255125749467 | 0.0021804444789886475
Precision int8 | 260 | 0.04103255125749467 | **0.0011799652576446533**



#### Conclusion
* Conversion with `int8` precision has best inference time.
* There is no loss in performance (MSE is same) of the optimized models.
* The model size has increased for optimized models.
* As we know there is no guarantee to receive better performing models with TensorRT, we can expect the results we have with PilotNet.

## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://www.tensorflow.org/lite/performance/model_optimization](https://www.tensorflow.org/lite/performance/model_optimization) \\
[5] [https://www.tensorflow.org/model_optimization/guide/install](https://www.tensorflow.org/model_optimization/guide/install) \\
[6] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[7] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) \\
[8] [https://www.tensorflow.org/model_optimization/guide](https://www.tensorflow.org/model_optimization/guide)
