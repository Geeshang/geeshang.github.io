---
layout: post
permalink: /convolution-computing
title:  卷积计算原理与实现解析
comments: true
date: 2018-02-27
---

 
(持续更新中)

## 1. 卷积概述
卷积操作(Convolution Operation)是卷积神经网络(CNN)的核心操作，本文从各种卷积操作原理进行介绍，着重计算部分，并配合开源深度学习框架的源码实现进行解析，源码实现解析的框架包括两种目前比较受欢迎的PyTorch以及TensorFlow.
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
[1] [Vincent Dumoulin et al. A guide to convolution arithmetic for deep learning](https://arxiv.org/pdf/1603.07285.pdf)

[2] [Goodfellow et al. Deeplearning book - Chapter 9](http://www.deeplearningbook.org/contents/convnets.html)

[3] [CS231n notes. Convolutional Neural Networks](http://cs231n.github.io/convolutional-networks/)