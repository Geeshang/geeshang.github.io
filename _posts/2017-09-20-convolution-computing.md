---
layout: post
permalink: /convolution-computing
title:  卷积计算原理与实现解析
comments: true
date: 2018-03-12
mathjax: true
---
 
(持续更新中)

## 1. 卷积概述
卷积操作(Convolution Operation)是卷积神经网络(CNN)的核心操作，本文从各种卷积操作原理进行介绍，着重计算部分，并简介几种卷积计算的实现方式，包括转换为矩阵相乘、快速傅里叶变换、直接计算. 

高效的卷积计算实现与底层硬件密切相关，基于CPU的实现由于运算过慢，在实际训练与检测场景中一般均不考虑；专用ASIC如Google的TPU、寒武纪的cambricon系列、深鉴科技的系列硬件方案，正在兴起，还未普及；目前在训练场景下大规模应用的是Nvidia的GPU. 

在Nvidia GPU硬件上一层的计算库方面，Nvidia推出的深度学习计算库cuDNN在R3版本以后，成为事实上卷积计算的最佳实现库. 目前流行的深度学习框架如TensorFlow、PyTorch、MXNet、Caffe，在GPU进行卷积计算都是调用cuDNN提供的API.

## 2. 卷积类型
本文介绍了5种卷积操作. 第1种是普通卷积，是最为常用的；第2、3种从感受野(receptive field)的范围的角度进行划分，包括transposed convolution、dilated Convolution，较常用；第4、5种从权值共享的方式的角度进行划分，包括unshared convolution、tiled convolution，较少用.
### 1. Odinary covolution
普通卷积操作，以2维卷积为例，有两个输入，输入数据 $D\in\mathbb{R}^{NC_{in}H_{in}W_{in}}$与过滤器(filter)或者叫卷积核(kernel) $F\in\mathbb{R}^{C_{in}C_{out}H_{k}W_{k}}$，一个输出，
输出数据 $O\in\mathbb{R}^{NC_{out}H_{out}W_{out}}$，都是4维的张量. 符号含义如下表所示：

符号|含义
---|---
N|mini-batch中图片的数量
$C_{in}$|输入通道的数量
$H_{in}$|输入图片（或特征图）的高度
$W_{in}$|输入图片（或特征图）的宽度
$C_{out}$|输出通道的数量
$H_{k}$|卷积核的高度
$W_{k}$|卷积核的宽度
$H_{out}$|输出特征图的高度
$W_{out}$|输入特征图的宽度
$pad\_h$|对高度的padding大小
$pad\_w$|对宽度的padding大小
$\sigma$|卷积步长（stride）

输出特征图的大小，由输入特征图的大小、卷积核的大小、padding的大小、步长stride共同决定，其长宽计算公式如下：

$$H_{out} = f(H_{in}, H_{k}, \sigma, pad\_h) = \left \lceil \frac{H_{in} - H_{k} + 1 + 2pad\_h}{\sigma} \right \rceil$$

$$W_{out} = f(W_{in}, W_{k}, \sigma, pad\_w) = \left \lceil \frac{W_{in} - W_{k} + 1 + 2pad\_w}{\sigma} \right \rceil$$

卷积计算时间复杂度：

$$O\left (H_{k} \cdot W_{k}  \cdot H_{out} \cdot W_{out} \cdot C_{in} \cdot C_{out} \right )$$

卷积计算空间复杂度：

$$O\left (H_{k} \cdot W_{k} \cdot C_{in} \cdot C_{out} \right )$$


### 2. Transposed convolution
### 3. Dilated convolution
### 4. Unshared convolution
### 5. Tiled convolution


## 3. 计算实现方式
### （1）转换为矩阵乘法
### （2）利用快速傅里叶变换
### （3）直接计算

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

[8] [He et al. Convolutional Neural Networks at Constrained Time Cost](https://arxiv.org/pdf/1412.1710.pdf)