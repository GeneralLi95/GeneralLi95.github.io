---
title: LeNet 笔记
date: 2019-12-10 19:19:26
tags:
  - 机器学习
  - 深度学习
  - 神经网络
categories:
  - 机器学习
mathjax:
---
## LeNet
论文提出，Yann LeCun 1998年  
[Gradient-Based Learning Applied to Document Recognition.pdf](http://yann.lecun.com/exdb/publis/pdf/lecun-01a.pdf)

网络结构
![LeNet](https://i.loli.net/2019/12/10/Do7F938hGXu2QJt.png)


![LeNet5 结构总结](https://i.loli.net/2019/12/19/LWsHUaEjAYxyd4b.png)
论文中采用的激活函数为 tanh，目前主流实现中的激活函数都是 ReLU

PyTorch 网络结构代码

```python
import torch.nn as nn
import torch.nn.functional as F

class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x
```

`__init__` 定义操作，`forward` 里是网络流程，拆分看就是

conv1 -> relu -> pool -> conv2 ->relu -> pool -> fc1 -> relu -> fc2 -> relu -> fc3

最初用于手写体字母识别，后被证明适合于图像分类。

**view的作用**

view相当于对一个tensor的reshape，典型的例子是

```python
import torch
a = torch.tensor(1, 16)
a = a.view(4,4)
```

**-1** 参数的作用，在不知道行有多少，但是知道列的情况下，可以用-1代替（但是注意要能整除），相当于告诉系统有多少列让系统自动帮你计算有多少行。
> If there is any situation that you don't know how many rows you want but are sure of the number of columns, then you can specify this with a -1. (Note that you can extend this to tensors with more dimensions. Only one of the axis value can be -1). This is a way of telling the library: "give me a tensor that has these many columns and you compute the appropriate number of rows that is necessary to make this happen".
> This can be seen in the neural network code that you have given above. After the line x = self.pool(F.relu(self.conv2(x))) in the forward function, you will have a 16 depth feature map. You have to flatten this to give it to the fully connected layer. So you tell pytorch to reshape the tensor you obtained to have specific number of columns and tell it to decide the number of rows by itself.




## 神经网络的几个层详解：
### 卷积层 conv
CNN的第一层一般是卷积层(conv)。一般采用 filter 也可称为 neuron 或者 kernel，我感觉叫 kernel好一点，卷积核。kernel 一般为 3*3 或者 5*5 大小的窗口，在整个图像进行滑动。这个窗口区域就是 CNN 的感受野(receptive field)， kernel 一般会对有一组数字对应（不是说kernel本身的矩阵)，也就是权重(weight)或者参数(parameters)。其中 kernel 的depth对应输入图像的depth。filter的结果对应 activetion map 或者feature map。

### 池化层 pool
用 feature map 作为输入，用不含参数的 filter (小方框自由设计尺寸)，对其进行扫描，平移，跳格。filter 没有参数，但是要对方框内的数值做做大值(max pooling)或均值(average pooling)保留，抹去方框内其他值，实现压缩浓缩；把流行的值形成的matrix 也就是压缩后的 feature map 传给下一层 hidden layer。
下采样也是一种池化。

池化的作用是减小计算量，减小内存消耗，提高感受野的大小，减少参数数量，增加平移不变性。

### 激活函数层 relu
激活函数 activation function，也叫激励函数，一般用relu作为激励函数

激励函数是为了增加神经网络的非线性，如果不使用非线性激活函数，则分类器永远是线性的，即使是在更多维的空间内也是线性的，而线性模型的表达能力是不够的。所以必须引入非线性激励函数，常用的非线性激励函数有sigmoid、tanh、ReLU。目前在深度学习中用的最多的就是 ReLU。

ReLU更容易学习优化，因为其分段线性的性质，导致其前传，后传，求导都是分段线性。而传统的 sigmoid 函数，由于两端饱和，在传播过程中容易丢弃信息。

Rectified Linear Units
![](https://i.loli.net/2019/12/10/dnZh3HFN7eVUbzq.png)
### 全连接层 fc
Fully Connected Layer，全连接层，简写为fc。在整个神经网络中起到「分类器」的作用。卷积层、池化层和激活函数层等操作是将原始数据映射到隐层特征空间，全连接层则起到将学到的「分布式特征表示」映射到样本标记空间的作用。

由于目前全连接层参数冗余(仅全连接层参数就可占整个神经网络参数80%左右)，一些性能优异的网络模型如 ResNet 和 GoogLeNet 等均采用全局平均池化(global average pooling, GAP) 取代 FC 来融合学到的深度特征，最后仍用 softmax 等损失函数作为网络目标函数来指导学习过程。需要指出的是，用 GAP 替代 FC 的网络通常有较好的预测性能。

即使 FC 越来越不被看好，有研究发现，FC 可在模型能力迁移过程中充当「防火墙」。如果目标域与源域图像差异较大，不含 FC 的网络微调后的结果差于含 FC 的网络。因此 FC 可以视为模型表示能力的「防火墙」，说明冗余的参数并非一无是处。



## 参考

1. [魏秀参：全连接层有什么用?](https://www.zhihu.com/question/41037974)
2. [stackoverflow:How does the "view" method work in PyTorch?](https://stackoverflow.com/questions/42479902/how-does-the-view-method-work-in-pytorch)
3. [LeNet-5 – A Classic CNN Architecture](https://engmrk.com/lenet-5-a-classic-cnn-architecture/)
