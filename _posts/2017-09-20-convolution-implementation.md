---
layout: post
permalink: /convolution-implementation
title:  卷积计算的底层实现简介
comments: true
date: 2018-07-10
mathjax: true
---
 
### （持续更新中）

## 1. 卷积概述
卷积操作(Convolution Operation)是卷积神经网络(CNN)的核心操作，对于CNN的网络结构的整体构成，你可能比较清楚，各种深度学习框架调得也很溜，不知道对卷积计算底层涉及到的实现方式是否清楚？本文给出几种卷积类型，给出相应的特征图大小计算，感受野计算公式，结合一些深度学习框架与计算库，简介几种卷积计算的实现方式，包括转换为矩阵相乘、快速傅里叶变换、直接计算.

高效的卷积计算与底层硬件密切相关，CPU上的卷积计算由于速度较慢，深度学习相关模型在训练阶段一般不予考虑，一些专用ASIC如Google的TPU、寒武纪的cambricon系列、深鉴科技的系列硬件方案，正在兴起，还未普及；目前在训练阶段下大规模应用的是Nvidia的GPU，检测阶段针对具体性能需求可能采用不同的硬件.

高效的卷积计算也和紧贴硬件的计算库有关，在Nvidia GPU硬件上的计算库方面，Nvidia推出的深度学习计算库cuDNN在R3版本以后，成为事实上卷积计算的最佳实现库，在此之前Facebook提出的基于快速傅里叶变换的库fbfft可以一战，还略胜一筹，不过随着nvidia对cudnn的不断优化，其他的计算库已经难望其项背了. 目前流行的深度学习框架如TensorFlow、PyTorch、MXNet、Caffe，在GPU进行卷积计算都是调用cuDNN提供的API.

## 2. 卷积类型
此处先介绍几种卷积操作，以及相应的特征图大小计算，后面介绍卷积实现原理要用到. 第1种是普通卷积，是最基本的；第2、3种从感受野(receptive field)范围角度相对普通卷积进行区分，包括transposed convolution、dilated Convolution，比较常见；第4、5种从权值共享的方式的角度进行区分，包括unshared convolution、tiled convolution，较少见.
### 1. Odinary covolution
普通卷积操作，以2维卷积为例，有两个输入，输入数据 $D\in\mathbb{R}^{NC_{in}H_{in}W_{in}}$与过滤器(filter)或者叫卷积核(kernel) $F\in\mathbb{R}^{C_{in}C_{out}H_{k}W_{k}}$，一个输出，
输出数据 $O\in\mathbb{R}^{NC_{out}H_{out}W_{out}}$，都是4维的张量. 符号含义如下表所示：

符号|含义
---|---
N|mini-batch中图片的数量
$C_{in}$|输入通道的数量
$H_{in}$|输入图片（或特征图）的高度
$W_{in}$|输入图片（或特征图）的宽度
$C_{out}$|输出通道的数量
$H_{k}$|卷积核的高度
$W_{k}$|卷积核的宽度
$H_{out}$|输出特征图的高度
$W_{out}$|输入特征图的宽度
$pad\_h$|对高度的padding大小
$pad\_w$|对宽度的padding大小
$\sigma$|卷积步长（stride）

#### 特征图输出大小计算

输出特征图的大小，由输入特征图的大小、卷积核的大小、padding的大小、步长stride共同决定，其长宽计算公式如下：

$$H_{out} = f(H_{in}, H_{k}, \sigma, pad\_h) = \left \lceil \frac{H_{in} - H_{k} + 1 + 2pad\_h}{\sigma} \right \rceil$$


这是`1`在里面取上界的计算公式，还有一种是`1`在外面取下界的方式：

$$H_{out} = f(H_{in}, H_{k}, \sigma, pad\_h) = \left \lfloor \frac{H_{in} - H_{k} + 2pad\_h}{\sigma} \right \rfloor + 1$$

上面是输出特征图高度的计算，宽度计算类比可以得出，实际中一般高宽是相等的。

#### 感受野（receptive field）的计算
$$RF_k = RF_{k-1} + (kernel - 1) \times \prod_{i=1}^{k-1}stride_i$$

其中$RF_{k-1}$代表上一个卷积层的感受野大小，$kernel$是本层卷积核大小。$\prod_{i=1}^{k-1}stride_i$代表之前所有层stride连乘积（不包含本层）。另外pooling层计算感受野时与卷积层是一样的，都有kernel与stride。

所谓感受野，就是这层卷积出来的每个特征点，有多大范围的输入图片信息影响到这个特征点。

看个例子，比如计算AlexNet的前几层特征图的感受野计算过程：

```
conv1: kernel = 11x11, stride = 4
pool1: kernel = 3x3, stride = 2
conv2: kernel = 5x5, stride = 1
pool2: kernel = 3x3, stride = 2
```
按照上述公式逐步计算感受野大小：

```
首先conv1作为为第一层其特征图就是他的kernel大小 :
conv1 = 11

接下来依次按照公式进行计算着 :
pool1 = 11+(2-1)x4=19
conv2 = 19+(5-1)x4x2=51 
pool2 = 51+(3-1)x8x1 = 67
```

#### 卷积计算时间复杂度：

$$O\left (H_{k} \cdot W_{k}  \cdot H_{out} \cdot W_{out} \cdot C_{in} \cdot C_{out} \right )$$

也就是说，每层卷积计算的时间复杂度，只和卷积核大小，卷积核数量（$C_{in}$ * $C_{out}$），输出的特征图大小有关，与输入特征图大小无关。

#### 卷积计算空间复杂度：

$$O\left (H_{k} \cdot W_{k} \cdot C_{in} \cdot C_{out} \right )$$

每层卷积，参数量都是在卷积核上了，bias太少了只有$C_{out}$个。


### 2. Transposed convolution
### 3. Dilated convolution
### 4. Unshared convolution
### 5. Tiled convolution


## 3. 计算实现方式
### （1）转换为矩阵乘法
卷积操作仅包含一系列乘加计算，可以转算成一个大的矩阵乘法进行计算，关于卷积计算的原理，它算了什么东西，此处不再赘述，一个直观化的理解回顾，可以看看这个CS231n公开课中的[动态JS图示](http://cs231n.github.io/convolutional-networks/)。

将卷积计算转换为矩阵乘法计算，你可能遇见过一个叫`im2col`的操作，这是caffe最开始采用的卷积计算的方式，输入图片与模型权重经过im2col分别形成两个大矩阵，进行矩阵计算时，CPU上调用Intel的MKL BlAS库，GPU上调用cublas库，后来也提供了直接调用cudnn卷积计算API的封装实现，计算完成后，调用`col2im` 把结果矩阵还原为多维向量（也即是tensor）。

利用`im2col`，将卷积计算转化为矩阵乘法计算，这个操作比较重要，后面要提到的cudnn的卷积实现原理虽然不是直接利用这个操作，但是在大方法上是一致的。比如我们以第一层卷积为例，来看看这是一个怎么的流程，一张ImageNet分类模型常采用的大小为224x224 RGB通道的图片, 第一层卷积核大小为`3x3`，输出通道为`32`，stride为`1`. 那么输入tensor `X` 的维度即为`1x3x224x224`（Nx$C_{in}$x$H_{in}$x$W_{in}$），第一层卷积模型权重tensor `W` 维度为`3x3x3x32`（$H_{k}$x$W_{k}$x$C_{in}$x$C_{out}$，第三个3是对应RGB输入的三个通道，调用深度学习框架时一般不指定，框架根据上层输出通道设定，如有疑问可以看看[[3]](http://cs231n.github.io/convolutional-networks/)），`im2col`操作的核心就是将这俩tensor进行组合变形，弄成二维矩阵，参考下图所示的过程，将`X` 一般弄成`(224x224)x(3x3x3)`（此处利用上述特征图计算公式，计算出输出特征图的宽高，($H_{out}$x$W_{out}$)x($C_{in}$x$H_{k}$x$W_{k}$)），`W` 弄成 `(32)x(3x3x3)`（($C_{out}$)x($C_{in}$x$H_{k}$x$W_{k}$)），这样做一次矩阵运算$WX^{T}$，得到`(32)x(224x224)`（($C_{out}$)x($H_{out}$x$W_{out}$)).

（待更新）

im2col对输入图片X与W操作图示


### （2）利用快速傅里叶变换
### （3）直接计算

## 参考文献
[1] [Dumoulin et al. A guide to convolution arithmetic for deep learning](https://arxiv.org/pdf/1603.07285.pdf)

[2] [Goodfellow et al. Deeplearning book - Chapter 9](http://www.deeplearningbook.org/contents/convnets.html)

[3] [CS231n notes. Convolutional Neural Networks](http://cs231n.github.io/convolutional-networks/)

[4] [Chetlur et al. cuDNN: Efficient Primitives for Deep Learning](https://arxiv.org/pdf/1410.0759.pdf)

[5] [Vasilache et al. Fast Convolutional Nets with fbfft: A GPU Performance Evaluation](https://arxiv.org/pdf/1412.7580.pdf)

[6] [hacker news comments over fbfft](https://news.ycombinator.com/item?id=10282903)

[7] [soumith/convnet-benchmarks](https://github.com/soumith/convnet-benchmarks)

[8] [He et al. Convolutional Neural Networks at Constrained Time Cost](https://arxiv.org/pdf/1412.1710.pdf)
