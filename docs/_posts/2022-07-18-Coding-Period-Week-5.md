---
title: "Coding Period: Week 5"
excerpt: "Result's video, more simulation and Collaborative Optimization"
usemathjax: true
sidebar:
  nav: "docs"

toc: true
toc_label: "Contents"
toc_icon: "cog"


categories:
- GSoC
tags:
- Video
- Collaborative Optimization
- DL Studio
- Tensorflow
- Behavior Metrics

author: Nikhil Paliwal
pinned: false
---


## Preliminaries
Previous week was focused on verifying the utility of the compressed models. We saw result's table of evaluation in a offline fashion (using usual test scripts) and via simulation with Behavior Metrics tool. This week I present the results in a more emersive method via video of F1 car covering a whole lap of the circuit. We can clearly see the inference (and the difference) on the screen. Furthermore, additional verification on difficult tracks such as Montreal and Montemelo will be performed. Next, we decide to explore more optimization strategies. One such group of strategies are called Collaborative Optimization, which combines multiple methods while preserving their properties. The models will be trained and evaluated for comparison. Finally, the TensorRT installation with Tensorflow (TF-TRT) will be targetted. 

## Objectives

- [X] Prepare video demonstration for PilotNet, Dynamic range quantized and Quantization aware trained models.
- [X] Do additional verification test (simulation) on Montemelo and Montreal circuits
- [X] Create scripts and optimize model for Cluster preservering quantization aware training (CQAT)(collaborative strategy)
- [X] Benchmark new optimized models in terms of `inference time`, `average speed` and `position deviation MAE` via offline and simulation methods.
- [X] Compare the simulation results and draw conclusions
- [X] Complete installation of Tensorflow-TensorRT (TF-TRT) on the server. 

### Additionally completed
- [X] New issues and solution for execution of TensorFlow supported brain on BehaviorMetrics 
- [X] Create scripts and optimize model for Sparsity preservering quantization aware training (PQAT)(collaborative strategy)
- [X] Create scripts and optimize model for Cluster and Sparsity preservering quantization aware training (PCQAT) (collaborative strategy)
  <!-- - [X] Structured pruning  -->

## Related Issues and Pull requests.

Related to use BehaviorMetrics repository:
* I face many the issue explained in the post - [docker vnc 2nd time connect issue](https://stackoverflow.com/questions/51633643/docker-vnc-2nd-time-connect-issue). I followed the solution proposed and it worked. Additionally, sometimes I have to set `$DISPLAY` variable to `:1` because `:0` was occupied, which I have to do in the container by editing `.bashrc`, `/etc/profile` and `/etc/bash.bashrc` and restart the container.

PR to sumbit new script:
* [Update the scripts to support optimized tflite models #386](https://github.com/JdeRobot/BehaviorMetrics/pull/386).
* [Add support for baseline evaluation and model optimization #67](https://github.com/JdeRobot/DeepLearningStudio/pull/67)

## The execution

### Video demonstration
 I used my personal computer with a `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU with `Intel® Core™ i7-7700HQ CPU @ 2.80GHz × 8 ` CPU, 8 GB RAM and batch size of 1 for simulation.

### PilotNet
<iframe width="560" height="315" src="https://www.youtube.com/embed/-7O53BGZKRc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Dynamic range quantization
<iframe width="560" height="315" src="https://www.youtube.com/embed/1C71Zyynvww" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Quantization aware training
<iframe width="560" height="315" src="https://www.youtube.com/embed/VlSD63_0yD4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://www.tensorflow.org/lite/performance/model_optimization](https://www.tensorflow.org/lite/performance/model_optimization) \\
[5] [https://www.tensorflow.org/model_optimization/guide/install](https://www.tensorflow.org/model_optimization/guide/install) \\
[6] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[7] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) \\
[8] [https://www.tensorflow.org/model_optimization/guide](https://www.tensorflow.org/model_optimization/guide)
