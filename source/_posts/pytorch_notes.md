---
title: PyTorch 笔记
date: 2019-12-19 15:03:17
tags:
  - PyTorch
categories:
  - Machine Learning
  - Deep Learning 
mathjax: True
---
## 1. 网络层参数理解
###  卷积层参数解释
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
* **bias** (bool,optional) – If True, adds a learnable bias to the output. Default: True 向输出添加可学习的偏置，默认 True

### 全连接层参数解释

```
torch.nn.Linear(in_features, out_features, bias=True)
```
对应公式：
$$y = x A^T + b$$

**in_features** - size of each input samples 输入的样本大小
**out_features** - size of each output samples 输出的样本大小
**bias** If set to False, the layer wiil not learn an additive bias. Default True如果设置为false, 图形不会学习偏差

## 2. Transforms in pytorch 手册阅读
>Data augmentation is the process of artificially enlarging your training dataset using carefully chosen transforms.
>When used appropriately, data augmentation can make your trained models more robust and capable of achieving higher accuracy without requiring larger dataset.
For those who are familiar with it, data augmentation is very similar to regularization in that it can prevent over-fitting compared to another identical model learning on the same dataset for the same number of epochs.
Two very useful transforms of this type that are commonly used in computer vision are random flipping and random cropping.
In torchvision, random flipping can be achieved with a random horizontal flip and random vertical flip transforms while random cropping can be achieved using the random crop transform.

Data augmentation 数据增强，人工一些精巧的变换来增强数据集。以前一直没把数据增强当回事，觉得是一些奇淫巧技，结果直到自己的 cifar 识别率一直达不到别人的同样水平，最后发现二者的差别就在于有没有用数据增强，加了一个随机水平翻转增强后最终准确率大大提高了有将近10个百分点(80% -> 90%)，看来这一块必须得重视。当成一个通用技巧来看。

Transforms 对图像进行变换，有各种方法

### 裁剪 Crop
* `torchvision.transforms.RandomCrop(size, padding=None, pad_if_needed=False, fill=0, padding_mode='constant')`
在随机位置裁剪原图像，
    * size 裁剪大小，两种结果，如果是 sequence，则为(h,w),高*宽，如果只有一个数字int，则裁剪为正方形。
    * padding，填充，可以是 sequence 或者 int。默认不填充，当为int时，上下左右同时填充，当有两个数的sequence时，第一个数表示左右填充值是多少，第二个说表示上下填充值是多少，当有四个数时，则分别为左、上、右、下（顺时针方向）
* `torchvision.transforms.RandomResizedCrop(size, scale=(0.08, 1.0), ratio=(0.75, 1.3333333333333333), interpolation=2)`
    *
### 翻转和旋转 Flip and Rotation
 * torchvision.transforms.RandomHorizontalFlip(p=0.5) 水平翻转图像，默认概率为0.5，决定这个图像是否被翻转
 * torchvision.transforms.RandomResizedCrop(size, scale=(0.08, 1.0), ratio=(0.75, 1.3333333333333333), interpolation=2)裁剪为随机大小和纵横比

### 标准化 Normalize
* torchvision.transforms.Normalize(mean, std, inplace=False)
对数据按通道进行标准化，即先减均值，再除以标准差。
$$input[channel1] = \frac{input[channel1] - mean[channel1]}{std[channel1]}$$
    * mean(sequence)，每个channel的均值
    * std(sequence)，每个channel的标准差


## 预训练模型下载地址

```
 1 Resnet:
 2 
 3 model_urls = {
 4     'resnet18': 'https://download.pytorch.org/models/resnet18-5c106cde.pth',
 5     'resnet34': 'https://download.pytorch.org/models/resnet34-333f7ec4.pth',
 6     'resnet50': 'https://download.pytorch.org/models/resnet50-19c8e357.pth',
 7     'resnet101': 'https://download.pytorch.org/models/resnet101-5d3b4d8f.pth',
 8     'resnet152': 'https://download.pytorch.org/models/resnet152-b121ed2d.pth',
 9 }
10 
11 inception:
12 
13 model_urls = {
14     # Inception v3 ported from TensorFlow
15     'inception_v3_google': 'https://download.pytorch.org/models/inception_v3_google-1a9a5a14.pth',
16 }
17 
18 Densenet:
19 
20 model_urls = {
21     'densenet121': 'https://download.pytorch.org/models/densenet121-a639ec97.pth',
22     'densenet169': 'https://download.pytorch.org/models/densenet169-b2777c0a.pth',
23     'densenet201': 'https://download.pytorch.org/models/densenet201-c1103571.pth',
24     'densenet161': 'https://download.pytorch.org/models/densenet161-8d451a50.pth',
25 }
26 
27 
28 
29 Alexnet:
30 
31 model_urls = {
32     'alexnet': 'https://download.pytorch.org/models/alexnet-owt-4df8aa71.pth',
33 }
34 
35 vggnet:
36 
37 model_urls = {
38     'vgg11': 'https://download.pytorch.org/models/vgg11-bbd30ac9.pth',
39     'vgg13': 'https://download.pytorch.org/models/vgg13-c768596a.pth',
40     'vgg16': 'https://download.pytorch.org/models/vgg16-397923af.pth',
41     'vgg19': 'https://download.pytorch.org/models/vgg19-dcbb9e9d.pth',
42     'vgg11_bn': 'https://download.pytorch.org/models/vgg11_bn-6002323d.pth',
43     'vgg13_bn': 'https://download.pytorch.org/models/vgg13_bn-abd245e5.pth',
44     'vgg16_bn': 'https://download.pytorch.org/models/vgg16_bn-6c64b313.pth',
45     'vgg19_bn': 'https://download.pytorch.org/models/vgg19_bn-c79401a0.pth',
46 }


```

## 参考

1. [stackoverflow: what is the what is the meaning of parameters involved in torch.nn.conv2d
](https://stackoverflow.com/questions/56675943/what-is-the-meaning-of-parameters-involved-in-torch-nn-conv2d)
2. [知乎:PyTorch 学习笔记（三）：transforms的二十二个方法](https://zhuanlan.zhihu.com/p/53367135)
3. [Augment the CIFAR10 Dataset Using the RandomHorizontalFlip and RandomCrop Transforms](https://www.aiworkbox.com/lessons/augment-the-cifar10-dataset-using-the-randomhorizontalflip-and-randomcrop-transforms)
4. [TorchVision 官方手册 Transforms 部分](https://pytorch.org/docs/stable/torchvision/transforms.html)
