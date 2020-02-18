---
title: 深度学习中的学习率变化和自适应学习率方法
date: 2019-11-20 10:30:29
tags:
  - Deep Learning
  - Learning Rate
categories:
  - Deep Learning
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

题外话，回调是要在训练过程的给定阶段应用的一组功能。 我们可以使用回调来获得训练期间模型的内部状态和统计信息。 在我们的示例中，我们通过扩展基类keras.callbacks.Callback来创建自定义回调，以记录训练过程中的丢失历史记录和学习率。

```
class LossHistory(keras.callbacks.Callback):
    def on_train_begin(self, logs={}):
       self.losses = []
       self.lr = []

    def on_epoch_end(self, batch, logs={}):
       self.losses.append(logs.get(‘loss’))
       self.lr.append(step_decay(len(self.losses)))
```

将所有内容放在一起，我们可以传递一个由LearningRateScheduler回调和我们的自定义回调组成的回调列表以适合模型。 然后，我们可以通过访问loss_history.lr和loss_history.losses可视化学习率进度表和损失历史记录。

```
loss_history = LossHistory()
lrate = LearningRateScheduler(step_decay)
callbacks_list = [loss_history, lrate]
history = model.fit(X_train, y_train,
   validation_data=(X_test, y_test),
   epochs=epochs,
   batch_size=batch_size,
   callbacks=callbacks_list,
   verbose=2)
```

![按步衰减模型精度](https://i.loli.net/2019/11/26/XRicvsu5gEfJwm6.png)

![按步衰减的学习率](https://i.loli.net/2019/11/26/lPLmMX1Jxfipy68.png)
### 1.4 指数衰减

指数衰减也是常用的学习率下降模式。数学公式是 `lr = lr0 * e^(−kt)`，这里`lr`,`k`都是超参数，`t`是循环次数。同样，我们可以通过定义指数衰减函数并将其传递给`LearningRateScheduler`来实现。 实际上，可以使用此方法在Keras中实现任何自定义衰减时间表。 唯一的区别是定义了不同的自定义衰减函数。

```
def exp_decay(epoch):
   initial_lrate = 0.1
   k = 0.1
   lrate = initial_lrate * exp(-k*t)
   return lrate
lrate = LearningRateScheduler(exp_decay)
```

![](https://i.loli.net/2019/11/26/5v24fC6RodxQXlb.png)

![](https://i.loli.net/2019/11/26/vnHbEkyPfSlG1jL.png)

现在让我们在示例中使用不同的学习率时间表来比较模型的准确性。

![](https://i.loli.net/2019/11/26/dzaVuJ9sirMDUgm.png)


## 2 自适应学习率方法
使用学习速率计划的挑战在于，必须提前定义其超参数，并且它们在很大程度上取决于模型和问题的类型。另一个问题是，将相同的学习率应用于所有参数更新。如果数据稀疏，则可能需要在不同程度上更新参数。

诸如Adagrad，Adadelta，RMSprop，Adam之类的自适应梯度下降算法提供了经典SGD的替代方法。这些按参数学习速率的方法提供了启发式方法，而无需为手动调整学习速率时间表的超参数而进行昂贵的工作。

简而言之，Adagrad对较大的稀疏参数执行较大的更新，对较小的稀疏参数执行较小的更新。它在稀疏数据和训练大规模神经网络方面表现良好。但是，当训练深度神经网络时，其单调学习率通常证明过于激进，并且过早停止学习。 Adadelta是Adagrad的扩展，旨在降低其激进的，单调降低的学习率。 RMSprop以非常简单的方式调整Adagrad方法，以尝试降低其激进的，单调降低的学习率。亚当是对RMSProp优化器的更新，类似于具有动力的RMSprop。

在Keras中，我们可以使用相应的优化器轻松实现这些自适应学习算法。通常建议将这些优化器的超参数保留为默认值（有时除外lr）。

```
keras.optimizers.Adagrad(lr=0.01, epsilon=1e-08, decay=0.0)
keras.optimizers.Adadelta(lr=1.0, rho=0.95, epsilon=1e-08, decay=0.0)
keras.optimizers.RMSprop(lr=0.001, rho=0.9, epsilon=1e-08, decay=0.0)
keras.optimizers.Adam(lr=0.001, beta_1=0.9, beta_2=0.999, epsilon=1e-08, decay=0.0)
```

现在让我们看看使用不同的自适应学习率方法的模型性能。 在我们的示例中，Adadelta提供了其他自适应学习率方法中最佳的模型精度。

![](https://i.loli.net/2019/11/26/PMzyi2NuXE5TrR9.png)

最后，我们比较了我们讨论过的所有学习率计划和自适应学习率方法的性能。

![](https://i.loli.net/2019/11/26/itTkIjezbpqcdRD.png)


## 结论

在我研究过的许多示例中，自适应学习率方法比学习率表显示出更好的性能，并且在超参数设置中需要更少的精力。 我们还可以使用Keras中的LearningRateScheduler创建针对我们数据问题的自定义学习率计划。

为了进一步阅读，Yoshua Bengio的论文为调整深度学习的学习速率提供了非常好的实用建议，例如如何设置初始学习速率，最小批量大小，时期数以及使用早期停止和动量。

[源代码](https://github.com/sukilau/Ziff-deep-learning/blob/master/3-CIFAR10-lrate/CIFAR10-lrate.ipynb)

## 参考文献

* [Practical Recommendations for Gradient-Based Training of Deep Architectures by Yoshua Bengio](https://arxiv.org/pdf/1206.5533v2.pdf)
* [Convolutional Neural Networks for Visual Recognition](http://cs231n.github.io/neural-networks-3/#sgd)
* [Using Learning Rate Schedules for Deep Learning Models in Python with Keras](https://machinelearningmastery.com/using-learning-rate-schedules-deep-learning-models-python-keras/)
