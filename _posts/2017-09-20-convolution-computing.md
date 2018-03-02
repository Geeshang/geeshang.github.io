---
layout: post
permalink: /convolution-computing
title:  卷积计算原理与实现解析
comments: true
date: 2018-02-27
---

 
(持续更新中)

## 1. 卷积概述
卷积操作(Convolution Operation)是卷积神经网络(CNN)的核心操作，本文从各种卷积操作原理进行介绍，着重计算部分，并给出几种卷积计算的实现方式，包括转换为矩阵相乘、快速傅里叶变换、直接计算. 高效的卷积计算实现与底层硬件密切相关，基于CPU的实现运算过慢，实际应用一般不考虑；专用ASIC如Google的TPU、寒武纪的cambricon系列，正在兴起，但目前还未大规模应用；现在大规模应用的是采用Nvidia的GPU进行加速，Nvidia推出的深度学习计算库cuDNN在R3版本以后，成为事实上的卷积计算的最佳实现. 目前流行的深度学习框架如TensorFlow、Pytorch、MXNet、Caffe底层卷积计算都是在cuDnn上的封装.

## 2. 卷积类型
本文介绍了5种卷积操作. 第1种是普通卷积，是最为常用的；第2、3种从感受野(receptive field)的范围的角度进行划分，包括transposed convolution、dilated Convolution，较常用；第4、5种从权值共享的方式的角度进行划分，包括unshared convolution、tiled convolution，较少用.
### 1. Odinary covolution
### 2. Transposed convolution
### 3. Dilated convolution
### 4. Unshared convolution
### 5. Tiled convolution


## 3. 计算方式
## 4. 深度学习框架实现
### pytorch
### tensorflow

## 参考文献
[1] [Dumoulin et al. A guide to convolution arithmetic for deep learning](https://arxiv.org/pdf/1603.07285.pdf)

[2] [Goodfellow et al. Deeplearning book - Chapter 9](http://www.deeplearningbook.org/contents/convnets.html)

[3] [CS231n notes. Convolutional Neural Networks](http://cs231n.github.io/convolutional-networks/)

[4] [Chetlur et al. cuDNN: Efficient Primitives for Deep Learning](https://arxiv.org/pdf/1410.0759.pdf)

[5] [Vasilache et al. Fast Convolutional Nets with fbfft: A GPU Performance Evaluation](https://arxiv.org/pdf/1412.7580.pdf)

[6] [hacker news comments over fbfft](https://news.ycombinator.com/item?id=10282903)

[7] [soumith/convnet-benchmarks](https://github.com/soumith/convnet-benchmarks)