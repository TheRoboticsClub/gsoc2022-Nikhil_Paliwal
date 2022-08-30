---
title: "Project Summary"
excerpt: "Project aim, contribution, videos, results, future work"
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

# Project Aim

The main aim of the project is to improve the current model stack of deep learning models, in terms of inference speed with minimum loss of precision, for autonomous driving applications. JdeRobot organization has created [Behavior Metrics](https://jderobot.github.io/BehaviorMetrics/), a tool for comparing deep learning architectures for autonomous driving on different circuits with the support of Gazebo and Ros Noetic. The organization also provide another tool called [DeepLearningStudio](https://jderobot.github.io/DeepLearningStudio/), which has datasets and model implementations for training deep learning models. I have used the available tools and techniques such as TensorRT, Quantization, Pruning, and varients to optimize the current model stack available in both PyTorch and TensorFlow framework.

# Contributions

The first three week for community bonding, involves a lot of working in solving issues and adding new features.
This initial work helped to set the base for further development and experimentation in coding period. The following issues and PR's are created and mergerd during and before that period.

* Issue [Replicating DeepestLSTMTinyPilotNet model from Tensorflow to PyTorch framework #37](https://github.com/JdeRobot/DeepLearningStudio/issues/37) solved by [Replicate DeepestLSTMTinyPilotNet model from Tensorflow to PyTorch framework #40](https://github.com/JdeRobot/DeepLearningStudio/pull/40)
* New feature - [Add support files for DeepestLSTMTinyPilotNet pytorch model #335](https://github.com/JdeRobot/BehaviorMetrics/pull/335)
* Created issue [Dependency conflict while installation #45](https://github.com/JdeRobot/DeepLearningStudio/issues/45) in DeepLearningStudio repo.
* Solved [issue #45](https://github.com/JdeRobot/DeepLearningStudio/issues/45) with PR [updated package versions for python3.10 #46](https://github.com/JdeRobot/DeepLearningStudio/pull/46) in DeepLearningStudio repo.
* Solved docker issues with PR [Fixes failing build of Docker images (with GPU support) in the workflow #365](https://github.com/JdeRobot/BehaviorMetrics/pull/365) in BehaviorMetric repo.
* I got errors while using `show_pilots.py` and created corresponding issue [KeyError while using show_plots.py script #366](https://github.com/JdeRobot/BehaviorMetrics/issues/366).
* Created another issue [Errors using 'scripts/analyse_brain.bash' #367](https://github.com/JdeRobot/BehaviorMetrics/issues/367).
* I encountered additional errors while using PilotNet (Pytorch) brain, for which I created additional issues - [Error while trying to save stats with DL-torch.yml config #368](https://github.com/JdeRobot/BehaviorMetrics/issues/368) and [Not utilizing GPU when running simulation #369](https://github.com/JdeRobot/BehaviorMetrics/issues/369).
* Create issue [Update PilotNet model to use new F1 dataset #48](https://github.com/JdeRobot/DeepLearningStudio/issues/48).
* Fixed the issue [Update PilotNet model to use new F1 dataset #48](https://github.com/JdeRobot/DeepLearningStudio/issues/48) by PR [Use new dataset #49](https://github.com/JdeRobot/DeepLearningStudio/pull/49). 
* Submitted PR [Removed: unnecessary exp details #370](https://github.com/JdeRobot/BehaviorMetrics/pull/370) to fix [issue #368](https://github.com/JdeRobot/BehaviorMetrics/issues/368).
* Solved [issue #369](https://github.com/JdeRobot/BehaviorMetrics/issues/369) by PR [Update: brain file to use gpu #371](https://github.com/JdeRobot/BehaviorMetrics/pull/371).
* Updated PR [updated package versions for python3.10 #46](https://github.com/JdeRobot/DeepLearningStudio/pull/46).
* Updated PR [Use new dataset #49](https://github.com/JdeRobot/DeepLearningStudio/pull/49).
* Create a PR [unnormalized prediction value #372](https://github.com/JdeRobot/BehaviorMetrics/pull/372) to unnormalize (expand) the predicted values of PilotNet brain in BehaviorNet.
* Updated PilotNet training script with PR [Adding validation set for model selection #50](https://github.com/JdeRobot/DeepLearningStudio/pull/50).


Please refer to following blog posts for more details:

[Community Bonding: Week 1](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Community-Bonding-Week-1/) <br>
[Community Bonding: Week 2](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Community-Bonding-Week-2/) <br>
[Community Bonding: Week 3](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Community-Bonding-Week-3/)


# Final simulation video

The following video provides an overview of the complete project and a comparison between the final models. All implementations, documentations and experiments are contributed to the two official repositories of the JdeRobot organization, i.e, [BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) and [DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio)  via Issues and Pull Requests (PRs). 

ADD YOUTUBE LINK (ADD TO MAIN PAGE ALSO)

## TensorFlow Framework

I started my work with TensorFlow framework. Till Phase 1 evaluation, the majority of goals for implementing and experimenting with 
optimized models were completed. The following video provide a overview on the performance.

<iframe width="560" height="315" src="https://www.youtube.com/embed/OcEoM9h2-5Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The intial weeks include setting up a baseline of available models for future comparison. Also, I worked on installing TensorRT for optimization. The following issues and PR's are created and mergerd during that period:

* Issue - [Missing argument: learning_rate #55](https://github.com/JdeRobot/DeepLearningStudio/issues/55)
* Issue - [Not utilizing GPU when running simulation #369](https://github.com/JdeRobot/BehaviorMetrics/issues/369) solved by
PR [Update: brain file to use gpu #371](https://github.com/JdeRobot/BehaviorMetrics/pull/371)

Please refer to following blog posts for more details:

* [Coding Period: Week 1](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-1/)
* [Coding Period: Week 2](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-2/)
* [Coding Period: Week 5](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-5/)

### TF Lite Optimization

All the optimization strategies are supported by TensorFlow Lite. There are majorly two categories - Quantization and Pruning.
Count the varients and combination of techniques, total 10 optimization strategies were implemented. Moreover, proper benchmarking was done in offline mode over testset (calculating Mean square error, modal size and inference time) and online model via live simulation on Gazebo.

The following issues and PR's are created and mergerd during that period:

* New feature - [Add support for baseline evaluation and model optimization #67](https://github.com/JdeRobot/DeepLearningStudio/pull/67)
* Issue - [ImportError: cannot import name 'ft2font' from 'matplotlib' #383](https://github.com/JdeRobot/BehaviorMetrics/issues/383)
* Issue - [AttributeError: 'Brain' object has no attribute 'suddenness_distance' #384](https://github.com/JdeRobot/BehaviorMetrics/issues/384)
* Issue - [Skipping registering GPU devices #385](https://github.com/JdeRobot/BehaviorMetrics/issues/385)
* PR (new feature) - [Update the scripts to support optimized tflite models #386](https://github.com/JdeRobot/BehaviorMetrics/pull/386).


Please refer to following blog posts for more details:

* [Coding Period: Week 3](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-3/)
* [Coding Period: Week 4](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-4/)
* [Coding Period: Week 5](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-5/)

Important Links:

* All trained models are available [here](https://drive.google.com/drive/u/1/folders/1j2nnmfvRdQF5Ypfv1p3QF2p2dpNbXzkt).
* All simulation videos are available [here](https://drive.google.com/drive/folders/1ovjuWjSy-ea7YtgnaSsgVsHnbo0HJY1A?usp=sharing)

### TensorFlow-TensorRT (TF-TRT)

[TensorRT](https://developer.nvidia.com/tensorrt) is an SDK for high-performance deep learning inference. It utilized various techniques such as precision calibration, layer fusion, kernel tuning etc. It also support TensorFlow via TensorFlow-TensorRT package. I have optimized the models in three different precision (Int 8, float 16 and float 32) and achieved excellent results.

The following issues and PR's are created and mergerd during that period:

* Issue - [Crash while recording stats with PilotNet (TF) model on Montreal circuit #392](https://github.com/JdeRobot/BehaviorMetrics/issues/392). 
* PR (new feature) - [Add support for inference optimization with TensorRT for Tensorflow models #71](https://github.com/JdeRobot/DeepLearningStudio/pull/71)
* PR (new feature) - [Add support for inference with TensorRT optimized (TF) models #395](https://github.com/JdeRobot/BehaviorMetrics/pull/395)
* Issue - [Crash while recording stats with PilotNet (TF) model on Montreal circuit #392](https://github.com/JdeRobot/BehaviorMetrics/issues/392)


Please refer to following blog posts for more details:

* [Coding Period: Week 5](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-5/)
* [Coding Period: Week 6](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-6/)
* [Coding Period: Week 7](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-7/)
* [Coding Period: Week 8](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-8/)
* [Coding Period: Week 9](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-9/)

Important Links:

* All trained models are available [here](https://drive.google.com/drive/u/1/folders/1j2nnmfvRdQF5Ypfv1p3QF2p2dpNbXzkt).
* All simulation videos are available [here](https://drive.google.com/drive/folders/1ovjuWjSy-ea7YtgnaSsgVsHnbo0HJY1A?usp=sharing)

## PyTorch Framework

PyTorch also support similar optimization strategies. There is support of TorchScript for inference and just changing the framework (from TF to PyTorch) show quite a performance difference. Most of the optimization strategies only support CPU inference, which could a major downside. I still received comparable and mostly better results with PyTorch. There are total 6 strategies implemented.

The following issues and PR's are created during that period:

* Issue - [AttributeError: 'Brain' object has no attribute 'suddenness_distance' #397](https://github.com/JdeRobot/BehaviorMetrics/issues/397) solved by [PR #399](https://github.com/JdeRobot/BehaviorMetrics/pull/399).
* Issue - [Issue using Pytorch quantized models #396](https://github.com/JdeRobot/BehaviorMetrics/issues/396)
* PR (new feature) - [PyTorch optimized model inference #399](https://github.com/JdeRobot/BehaviorMetrics/pull/399)
* PR (new feature) - [Implemented Pytorch optimization strategies #72](https://github.com/JdeRobot/DeepLearningStudio/pull/72)

Please refer to following blog posts for more details:

* [Coding Period: Week 8](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-8/)
* [Coding Period: Week 9](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-9/)
* [Coding Period: Week 10 & 11](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-10n11/)
* [Coding Period: Week 12](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Coding-Period-Week-12/)


Important Links:

* All trained models are available [here](https://drive.google.com/drive/folders/1qTQ8Fc7OqBElU8M2llO_P-8FaA7yhiP5?usp=sharing)
* All simulation videos are available [here](https://drive.google.com/drive/folders/1tRxVsPc0IGK2gm0gXYwy0R_mgEnHWzPp?usp=sharing).

# Results

I have perform an extensive range of experiments on each optimization strategy and compiled the combined results in a separate blog page - [Results Summary](https://theroboticsclub.github.io/gsoc2022-Nikhil_Paliwal/gsoc/Results-Summary/).

## Summarizing the improvements

The essential observations and improvements are presented below that I have achieved through the project. For exact numerical figures, please see the result page (mentioned above). For code implementation, please refer to the individual PRs.

### TensorFlow framework

* We achieved a **~12x** reduction in the model memory size with `Dynamic range quantization`.
* We maintain a similar `MSE` value (at best 0.001 better) as baseline in offline evaluation.
* We achieved a **~33x** better inference time with `TensorRT Int8` optimization  and **~7.5x** better inference time with `Dynamic range quantization` in offline evaluation.
* We achieved **~0.66x** times smaller `Position deviation MAE` and **~12x** time higher Brain iteration frequency (RT) in simulation.
* We achieved **~22x** time improvement in `Mean inference time` in simulation.

### PyTorch framework

* We achieved, at best, a **~4x** memory reduction with `Static Quantization technique`.
* The `MSE` is improved by 0.011 from baseline using `Prune + Quantization`. Other techniques also 
gives a slight better performance.
* All the methods inference time in same scale ( 10 <sup>-3</sup> ) of magnitude.
`Local prune` strategy has the best inference speed (cpu). 
* `Quantization + Prune` gives the best performance overall. However, all other
strategies are also very close.

## Recommendations

* PyTorch optimized models are smaller in size and have better inference time. I would recommend `Global/Local Prune` strategy for start because they also support inference with GPU. `Quantization + Prune` has least `MSE` but only support CPU inference.
* Tflite optimized models gives better performance than original models with very less memory size. The installation is easy and there is no specific hardware constraints. I would recommend `Dynamic range quantization` as first optimization method.
* TensorRT optimized models have best performance in both offline and simulation. However, they have large memory footprint. If the disk space is not a constraint, I would recommend using `Int8` or `Float16` precision model.

# Future work

TensorRT also supports PyTorch framework with [Torch-TensorRT](https://pytorch.org/TensorRT/) compiler. It would be interesting to compare the same TensorRT optimizations between the two popular deep learning frameworks (TensorFlow and PyTorch). We already have everything setup for offline and online benchmarking, which could provide additional insights for our project. I have saved useful resources for installation and development as follow:

* [Torch-TensorRT Installation](https://pytorch.org/TensorRT/getting_started/installation.html#installation)
* [Torch-TensorRT Documentation](https://pytorch.org/TensorRT/)
* [Blog - How to Convert a Model from PyTorch to TensorRT and Speed Up Inference](https://learnopencv.com/how-to-convert-a-model-from-pytorch-to-tensorrt-and-speed-up-inference/)  