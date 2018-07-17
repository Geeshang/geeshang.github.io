---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: Geeshang's Blog
permalink: /
---

## 一、深度学习基础解析
这个系列会做一些深度学习基础技术上的分享，包括CNN(卷积神经网络)与RNN(递归神经网络)相关原理。

1. ### [解读CNN网络结构进化之路](/cnn-evolution)
    从CNN鼻祖LeNet5到掀起深度学习浪潮的AlexNet，CNN发展到今天，出现了众多网络结构，诸如VGG、GoogleNet、ResNet、DenseNet、ResNext。这些网络结构都是怎样的，他们之间有哪些联系与区别，本文将梳理CNN的发展脉络，解读其进化之路。

3. ### [卷积计算底层实现简介](/convolution-implementation)
    卷积操作(Convolution Operation)是卷积神经网络(CNN)的核心操作，对于CNN的网络结构的整体构成，你可能比较清楚，各种深度学习框架调得也很溜，不知道对卷积计算底层涉及到的实现方式是否清楚？本文给出几种卷积类型，给出相应的特征图大小计算，感受野计算公式，结合一些深度学习框架与计算库，简介几种卷积计算的实现方式，包括转换为矩阵相乘、快速傅里叶变换、直接计算. 

---

## 二、目标检测、语义分割、实例分割
这个系列会针对计算机视觉领域中，目标检测（Object Detecion）、语义分割（Semantic Segmentation）与实例分割（Instance Segmentation）这三个应用领域，做一些技术上的分享。

1. ### [解读R-CNN魔改之路：从R-CNN到Mask R-CNN](/rcnn)
    从R-CNN、Fast R-CNN、Fater R-CNN到Mask R-CNN，这一路算是在深度学习时代目标检测（object detection）的里程碑式的论文，是two-stage这个分支的代表作。R-CNN怎么就越来越fater了？Mask R-CNN添加一个分支就把目标检测与实例分割（instance segmentation）联系起来了？本文将梳理R-CNN这一支的发展历程，解读其魔改之路。

---

## 三、模型压缩

---