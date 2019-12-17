---
title: VGG 笔记
date: 2019-12-12 17:24:16
tags:
  - 机器学习
  - 深度学习
  - 神经网络
categories:
  - 机器学习
mathjax:
---
## VGG Net

论文提出 Oxford Visual Geometry Group 2014年

[Very Deep Convolutional Networks for Large-Scale Image Recognition](https://arxiv.org/pdf/1409.1556.pdf%20http://arxiv.org/abs/1409.1556.pdf)
![VGG16](https://i.loli.net/2019/12/12/ovMdYrGBi5Kf8UP.png)

VGG 相比AlexNet的一个改进是采用连续的几个 3x3 的卷积核代替 AlexNet 中的较大卷积核(11x11，7x7， 5x5)。对于给定的感受野(与输输出有关的输入图片的局部大小)，采用堆积的小卷积核是由于采用大的卷积核，因为多层非线性层可以增加网络深度来保证学习更复杂的模式，而且代价还更小(参数更少)。


下面这张图解释了为什么使用2个3x3的卷积核可以代替5x5的卷积核。
![](https://i.loli.net/2019/12/15/5DX4at7R18HwAJ2.png)


## VGG优缺点

### VGG优点
* VGGNet 的结构非常简洁，整个网络都使用了同样大小的卷积核尺寸(3x3)和最大池化尺寸(2x2)
* 几个小滤波器(3x3)卷积层的组合比一个大滤波器(5x5)或(7x7)卷积层更好
* 验证了通过不断增加网络结构可以提高性能

### VGG缺点
* VGG耗费更多计算资源，并且使用了更多的参数，导致更多的内存占用(140M)。其中绝大多数参数来自于第一个全连接层。
* VGG参数空间很大，最终的model有500M+，而AlexNet只有200M，GoogLeNet更少，所以训练一个VGG需要更长的时间。
## 参考

[Quora: What is the VGG neural network](https://www.quora.com/What-is-the-VGG-neural-network)


[Paper: Rethinking the Inception Architecture for Computer Vision](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Szegedy_Rethinking_the_Inception_CVPR_2016_paper.pdf)

[知乎: 一文读懂VGG网络](https://zhuanlan.zhihu.com/p/41423739)
