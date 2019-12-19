---
title: PyTorch 笔记
date: 2019-12-19 15:03:17
tags:
  - PyTorch
categories:
  - 机器学习
mathjax: True
---
## 卷积层参数解释
```
torch.nn.Conv2d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True, padding_mode='zeros')
```
* **in_channel**(int) - Number of channels in the input image，输入图片通道，RGB图片3通道
* **out_channels** (int) – Number of channels produced by the convolution, 卷积的输出通道，表示有多少个 filter 对输入进行滤波，同时输出N个结果
* **kernel_size** (int or tuple) - Size of the convolving kernel 卷积核的大小
* **stride**(int or tuple, optional) - Stride of the convolution(Default 1) 卷积核滑动的步幅，默认1
* **padding**(int or tuple,optional) - Zero-padding added to both sides of the input. Default: 0，输入两侧填充的0的个数
* **dilation**(int or tuple, optional) - Spacing between kernel elements. Default: 1，内核元素之间的间距，默认1
* **groups** (int,optional) – Number of blocked connections from input channels to output channels. Default: 1 从输入通道到输出通道的阻塞连接数，默认1
* **bias** (bool,optional) – If True, adds a learnable bias to the output. Default: True 向输出添加可学习的偏见，默认 True

## 全连接层参数解释

```
torch.nn.Linear(in_features, out_features, bias=True)
```
对应公式：
$$y = x A^T + b$$

**in_features** - size of each input samples 输入的样本大小
**out_features** - size of each output samples 输出的样本大小
**bias** If set to False, the layer wiil not learn an additive bias. Default True如果设置为false, 图形不会学习偏差





## 参考

[stackoverflow: what is the what is the meaning of parameters involved in torch.nn.conv2d
](https://stackoverflow.com/questions/56675943/what-is-the-meaning-of-parameters-involved-in-torch-nn-conv2d)
