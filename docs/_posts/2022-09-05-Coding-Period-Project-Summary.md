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

## Project Aim
FROM ROADMAP

## Contributions
MENTION BLOG LINKS

### TensorFlow Framework
ADD MIDTERM VIDEO LINK

#### TF Lite Optimization

#### TensorFlow-TensorRT (TF-TRT)

### PyTorch Framework

## Final simulation video

ADD YOUTUBE LINK (ADD TO MAIN PAGE ALSO)

## Results

ADD BLOG LINK

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

## Future work

ADD LINKS FOR PYTORCH TENSORRT