---
title: 深度学习中的学习率变化和自适应学习率方法
date: 2019-11-20 10:30:29
tags:
  - 深度学习
  - 学习率
categories:
  - 深度学习
mathjax: true
---


声明：本文主要是以翻译Suki Lau的在Media博文 [Learning Rate Schedules and Adaptive Learning Rate Methods for Deep Learning](https://towardsdatascience.com/learning-rate-schedules-and-adaptive-learning-rate-methods-for-deep-learning-2c8f433990d1) 为基础，增加了个人的理解。

---

学习率 learning rate 是深度中最重要的超参数之一，学习率大小决定了神经网络收敛的速度，学习率太大，可能导致收敛速度过快而震荡，学习率太小则可能导致收敛速度太慢。
![学习率](https://i.loli.net/2019/11/20/jpqVeSfFwrgkBO5.png)

所以在我们训练神经网络的时候，经常在刚开始采用较大的学习率，随着时间推进，学习率会逐渐减小。我们可以预设一个公式或自适应的学习率变化方法(learning rate schedules or adaptive learning rate methods)。本文在 CIFAR-10 数据集上训练一个神经网络，通过使用不同的学习率变化来比较他们之间的表现差别。

## 1 预设学习率变化
学习率时间表通过预设一个学习率变化的时间表来控制学习率的衰减。通常的方法包括**时序衰减**，**按步衰减**，**指数衰减**。为了证明，我们构建了一个训练在 CIFAR-10 数据集上的卷积神经网络，用随机梯度下降方法(SGD)来表现不同学习率时间表的性能。

### 1.1 固定学习率

固定学习率方法就是设置一个默认的学习率。在 Keras 的 SGD optimizer 中实现。动量和衰减率都设置为 0。 在这种情况下，选择一个合适的学习率是不容易的。这需要一定的经验和大量的实验。在这个例子中，学习率 `lr = 0.1` 展示了一个相对较好的结果。这可以作为这篇文章实验的一个基线。

```
keras.optimizers.SGD(lr=0.1, momentum=0.0, decay=0.0, nesterov=False)
```

![固定学习率](https://i.loli.net/2019/11/21/k2CWzNHb3vnlQoV.png)

### 1.2 时序衰减学习率
时序衰减学习率的数学公式是 $lr=lr_0/(1 + k_t)$ 这里 $lr$，和 $k$ 都是一个高参数，$t$ 是一个迭代次数。如果看 Keras 的源码，会发现 SGD optimizer 把衰减和学习率的更新通过在每个周期递减一个参数数值来更新。

```
lr *= (1. / (1. + self.decay * self.iterations))
```

动量是 SGD optimizer 里的另外一个参数，我们可以通过调整它来获得更快的收敛速度。不像经典的 SGD 方法，动动量法可帮助参数矢量以恒定的梯度下降沿任意方向建立速度，从而防止振荡。一般动量选择的值是0.5-0.9之间。

SGD optimizer 还有一个参数叫做 nesterov，默认情况下它的值是 false. Nesterov 动量是动量方法的一种变形，对凸函数具有更强的理论收敛保证。在实际操作中，它的效果比标准动量方法稍好。

在 Keras 里我们可以实现时序衰减学习率通过在SGD optimizer 中设置初始学习率，衰减率和动量。

```
learning_rate = 0.1
decay_rate = learning_rate / epochs
momentum = 0.8
sgd = SGD(lr=learning_rate, momentum=momentum, decay=decay_rate, nesterov=False
```



### 1.3 按步衰减
按步衰减学习率通过一个参数，每几个epoch下降一次。公式为:

```
lr = lr0 * drop^floor(epoch / epochs_drop)
```
一个常用降低学习率的方法是每隔10个 epoch，学习率降低为原来的一半。在 Keras 中实现这个方法，我们可以定义一个逐步递减函数，使用 LearningRateScheduler 回调，将逐步衰减函数作为参数，并将更新的学习率返回到 SGD optimizer中。

```python
def step_decay(epoch):
   initial_lrate = 0.1
   drop = 0.5
   epochs_drop = 10.0
   lrate = initial_lrate * math.pow(drop,  
           math.floor((1+epoch)/epochs_drop))
   return lrate
lrate = LearningRateScheduler(step_decay)
```


### 1.4 指数衰减


## 2 自适应学习率方法



## 结论
