---
title: Andrew Ng Machine Learning - 8 Neural Networks: Representation
date: 2020-03-11 17:28:21
tags:
  - Andrew Ng Machine Learning
  - MOOC
categories:
  - Machine Learning
  - Stanford Andrew Ng Machine Learning Course
mathjax: True
---
终于学到了激动人心的神经网络。接下来的两章都是关于神经网络。
## 8.1 Motivations
### 8.1.1 Non-linear Hypotheses
神经网络的动机，为什么要使用神经网络。

非线性分类，如果采用多项式做法，feature将会非常多，而 feature 组合产生的特征会更加多。

Ng 举了一个例子，计算机视觉如何识别物体，如何识别照片里面是不是汽车，对于计算机来说，图片就是像素值矩阵。对于 50x50的图像，2500个像素，如果使用灰度图(非RGB)对于平方特征 $x_j \times x_j$，将会有3 million 个 features. 计算方法是 $n^2/2$

**quiz**
![](https://i.loli.net/2020/03/11/tTylDqKEYHU4bJd.png)
## 8.2 Neural Networks
### 8.2.1 Model Representation I
首先从生物学角度讲了神经网络。解释了为什么命名为神经网络。在哔哩哔哩上面老版的课程这一节前面还有一个单独的一节叫「神经元与大脑」。

> parameters 参数
> weights 权重
> 在神经网络中，经常用 weights 来表示 parameters

第一层，也叫输入层，最后一层，叫输出层。中间层叫隐含层(hidden layer)。隐含层可能不只有一层，只要不是输入层和输出层就都是隐含层。

> activation 激励
![](https://i.loli.net/2020/03/11/DoflSUFPqQnxN5Z.png)

**quiz**
第一层两个节点(不包含bias几点)，第二层四个节点，问dimension of $\Theta$
**Answer 4x(2+1) = 4x3**
### 8.2.2 Model Representation II
> propagation

这一届将介绍一下前向传播的向量化实现。前向传播，就是从输入层，按照网络结构，一路推导到输出层的过程。

如果将输入层挡住，只看隐含层到输出层部分，会像一个逻辑回归单元，只不过这里的 feature 是由初始 feature 而来。


## 8.3 Applications
