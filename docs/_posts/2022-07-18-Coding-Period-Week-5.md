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
* I face many the issue explained in the post - [docker vnc 2nd time connect issue](https://stackoverflow.com/questions/51633643/docker-vnc-2nd-time-connect-issue). I followed the solution proposed and it worked. Additionally, sometimes I have to set `$DISPLAY` variable to `:1` because `:0` was occupied, which I have to do in the container by editing `.bashrc`, `/etc/profile` and `/etc/bash.bashrc` and restart the container. The end solution was to remove `/tmp/.X0-lock` file in running container and restart it.

PR to sumbit new script:
* [Update the scripts to support optimized tflite models #386](https://github.com/JdeRobot/BehaviorMetrics/pull/386).
* [Add support for baseline evaluation and model optimization #67](https://github.com/JdeRobot/DeepLearningStudio/pull/67)

## The execution

### Video demonstration
 I used my personal computer with a `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU with `Intel® Core™ i7-7700HQ CPU @ 2.80GHz × 8 ` CPU, 8 GB RAM and batch size of 1 for simulation.

#### PilotNet
<iframe width="560" height="315" src="https://www.youtube.com/embed/-7O53BGZKRc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### Dynamic range quantization
<iframe width="560" height="315" src="https://www.youtube.com/embed/1C71Zyynvww" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### Quantization aware training
<iframe width="560" height="315" src="https://www.youtube.com/embed/VlSD63_0yD4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Additional performance benchmarking on simulation (online fashion)

I used my personal computer with a `NVIDIA GeForce GTX 1050/PCIe/SSE2` GPU with `Intel® Core™ i7-7700HQ CPU @ 2.80GHz × 8 ` CPU, 8 GB RAM and batch size of 1 (for inference). The `stats` are recorded after approximately one lap or untill collision in `Montreal circuit` and for comparison, focus should be on the aspect of average calculations. Unfortunately, the F1 car collide with the wall at very early stage in `Montemelo circuit`, so I have not include their stats (since they will not be reliable).

#### Comparison table - Montreal Circuit


Method  | Average speed | Mean Inference time (s) | Distance covered | Circuit complete 
--- | --- | --- | --- | ---
PilotNet (original) | 9.431 | 0.1228 | 1301.58 | No
Dynamic Range Q | 9.728 | 0.0119 | 1303.83 | No
Q aware training | **10.06** | **0.0114** | 2737 | **Yes**

#### Conclusion
* The optimized models are better in terms of MSE performance than original models.
* All the models still struggle with difficult circuits and better training strategies are need for this.
* On average, quantization strategy gives a boost of ~10x times to other aspects (inferece time etc.).
* We observe slight improvement in `Average speed` and `Position deviation MAE`. The credit can be given to reduced latency by optimized models.

### [Collaborative Optimization](https://www.tensorflow.org/model_optimization/guide/combine/collaborative_optimization)

The idea of collaborative optimizations is to build on individual techniques by applying them one after another to achieve the accumulated optimization effect. The issue that arises when attempting to chain these techniques together is that applying one typically destroys the results of the preceding technique, spoiling the overall benefit of simultaneously applying all of them. To solve this problem, Tensorflow has introduce the various experimental collaborative optimization techniques. I implemented and trained the following:
* Sparsity preserving quantization aware training (PQAT)
* Cluster preserving quantization aware training (CQAT)
* Sparsity and cluster preserving quantization aware training (PCQAT)

The updates in scripts are included in [PR#67](https://github.com/JdeRobot/DeepLearningStudio/pull/67). For using the complete dataset, I need a more powerful machine. I used a Nvidia V100 GPU with 32GB memory. The batch size was 1024. All subsets of new datasets are used for experiment.

#### Result table

Method | Model size (MB) | MSE | Inference time (s)
--- | --- | --- | --- 
PilotNet (original tf format) | 195 | 0.041 | 0.0364
Baseline (tflite format)| 64.9173469543457 | 0.04108056542969754 | 0.007913553237915039 
CQAT | 16.250564575195312 | 0.0393811650675438 | 0.007680371761322021
PQAT | 16.250564575195312 | 0.043669467093106665 | 0.007949142932891846
PCQAT | 16.250564575195312 | **0.039242053481006144** | 0.007946955680847167

#### Observations
* The new strategies have better MSE than all the models till now.
* The inference time boost is not the best but still better than original model.
* Unfortunately, none of the models were able to complete a lap in simulation.

### [Tensorflow TensorRT (TF-TRT)](https://github.com/tensorflow/tensorrt) Installation

I wanted to use a direct (uncomplicated) method for installation, so that anyone can follow it. After due research and trails for hours, I found one method which worked and it is also easy to use.

The blog - [TENSORRT INSTALLATION & OPTIMIZATION](https://www.sagarmandiya.me/blog/tensorrt-installation-&-optimization) present the steps (using conda and pip) to install tensorrt. Before starting, we need to install `miniconda`, for which we can refer to official documentation - [Installing on Linux](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html). After successful install, I wrote a test script to optimize ResNet-50 model and it worked!!


## References

[1] [https://github.com/JdeRobot/BehaviorMetrics](https://github.com/JdeRobot/BehaviorMetrics) \\
[2] [https://github.com/JdeRobot/DeepLearningStudio](https://github.com/JdeRobot/DeepLearningStudio) \\
[3] [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt) \\
[4] [https://www.tensorflow.org/lite/performance/model_optimization](https://www.tensorflow.org/lite/performance/model_optimization) \\
[5] [https://www.tensorflow.org/model_optimization/guide/install](https://www.tensorflow.org/model_optimization/guide/install) \\
[6] [https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html) \\
[7] [https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) \\
[8] [https://www.tensorflow.org/model_optimization/guide](https://www.tensorflow.org/model_optimization/guide)
