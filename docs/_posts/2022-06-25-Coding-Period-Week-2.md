---
title: "Coding Period: Week 2"
excerpt: "More on baseline and optimization experiment with TensorRT"
usemathjax: true
sidebar:
  nav: "docs"

toc: true
toc_label: "Contents"
toc_icon: "cog"


categories:
- GSoC
tags:
- Baseline
- TensorRT
- Behavior Metrics
- DL Studio

author: Nikhil Paliwal
pinned: false
---


## Preliminaries

During our meetings we discussed the possible reasons for previous week error. We also discussed about methods to calculate inference time. We have the options to use `time()` or `perf_counter()` method from `time` library [5] or follow the [blog](https://towardsdatascience.com/the-correct-way-to-measure-inference-time-of-deep-neural-networks-304a54e5187f) [4]. The blog provide more information about aspects of accurately measuring, asynchronous execution, GPU power-saving modes etc. After complete re-installation of cuda, I again start working on benchmarking baseline models. More work on previous PR was done. Moreover, I prepared scripts to evaluate PilotNet and DeepestLSTMTinyPilotNet.

## Objectives

- [X] Update previous PR's according to requested changes.
- [X] PilotNet baseline - Calculate loss (regression), inference time (script and BehaviorNet) 
- [X] DeepestLSTMTinyPilotNet baseline - Calculate loss (regression), inference time (script and BehaviorNet)
- [X] Installation of TensorRT
- [X] Decide on the method for inference benchmark.

## Related Issues and Pull requests.
* https://github.com/JdeRobot/BehaviorMetrics/pull/371

## The execution
I follow the data split defined in paper [Memory based neural networks for end-to-end autonomous driving](https://arxiv.org/abs/2205.12124). The baseline was calculate using dataset (total images - 94348) based on circuits - Simple circuit, Montmeló and Montreal. The experiments are performed using a `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU with `Intel® Core™ i7-7700HQ CPU @ 2.80GHz × 8 ` CPU, 8 GB RAM and batch size of 1.

### Baseline experiment

The following table present baseline performances:

Model | Loss | MSE | Mean Absolute Error | Inference time (`time()` / `perf_counter()` / [4])
--- | --- | --- | --- | ---
PilotNet | 0.041 | 0.041 | 0.095 | (0.0364 / 0.035 / 0.0323)  sec 
Deepest <br> LSTM <br> TinyPilotNet | 0.025 | 0.019 | 0.051 | (0.0358 / 0.0356 / 0.0355) sec

My code for evaluating the performance of PilotNet can be found in a separate branch [baseline-exp](https://github.com/nik1806/DeepLearningStudio/tree/baseline-exp). 

Since the inference time from all different methods are quite close. We decided to stick to default `time()` method with a warm-up for our inference benchmark, similar to [9](https://colab.research.google.com/drive/1Cv0U4URoI2zQ7NufK8FtLNcB_KPphfhB?usp=sharing).

## TensorRT installation

TensorFlow-TensorRT (TF-TRT) is a library to support inference optimization on Nvidia GPU's. Nvidia has very well presented it in the [blog](https://blog.tensorflow.org/2021/01/leveraging-tensorflow-tensorrt-integration.html) [6, 7]. Two important things which needs to taken care:
1. TensorFlow 2.x, TF-TRT only supports models saved in the TensorFlow [SavedModel](https://www.tensorflow.org/guide/saved_model) format.
2. TensorRT execution engine should be built on a GPU of the same device type as the one on which inference will be executed as the building process is GPU specific.

Additional steps for installations are:
```
pip install nvidia-pyindex
pip install nvidia-tensorrt
```

I came across an error `Could not load dynamic library 'libnvinfer.so.7'; dlerror: libnvinfer.so.7:` and `Could not load dynamic library 'libnvinfer_plugin.so.7'; dlerror: libnvinfer_plugin.so.7:`. Solving / installation will takes hours, so one should plan accordingly. I realized that unfortunately for local installation I have install specific version of cuda and TensorRT but some were not supported for my OS - ubunt18.04. After spending one day experimenting with different installation ways such as .deb, tars or pip wheel, I decided to use pre-build docker. Nvidia provide a docker image which support TensorRT and TF-TRT - [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow). 

To pull the docker into local machine:
```
docker pull nvcr.io/nvidia/tensorflow:22.06-tf2-py3
```

To run launch the container with DeepLearningStudio as mount:
```
docker run --gpus all -it --rm -v DeepLearningStudio/:DeepLearningStudio/ nvcr.io/nvidia/tensorflow:22.05-tf2-py3
```



## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://towardsdatascience.com/the-correct-way-to-measure-inference-time-of-deep-neural-networks-304a54e5187f](https://towardsdatascience.com/the-correct-way-to-measure-inference-time-of-deep-neural-networks-304a54e5187f) \\
[5] [https://stackoverflow.com/questions/52222002/what-is-the-difference-between-time-perf-counter-and-time-process-time](https://stackoverflow.com/questions/52222002/what-is-the-difference-between-time-perf-counter-and-time-process-time) \\
[6] [https://blog.tensorflow.org/2021/01/leveraging-tensorflow-tensorrt-integration.html](https://blog.tensorflow.org/2021/01/leveraging-tensorflow-tensorrt-integration.html) \\
[7] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[8] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) \\
[9] [Optimize TensorFlow Models For Deployment with TensorRT](https://colab.research.google.com/drive/1Cv0U4URoI2zQ7NufK8FtLNcB_KPphfhB?usp=sharing)