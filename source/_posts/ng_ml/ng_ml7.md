---
title: Andrew Ng Machine Learning - 7 Regularization
date: 2020-03-11 00:15:23
tags:
  - Andrew Ng Machine Learning
  - MOOC
categories:
  - Machine Learning
  - Stanford Andrew Ng Machine Learning Course
mathjax: True
---
这一章开始介绍过拟合问题，和解决过拟合问题的 Regularization 方法。
## 7.1 The Problem of Overfitting
> Regularization 正则化
> Normalization 归一化

欠拟合 underfit -> 较大误差 high bias
过拟合 overfit -> 较大方差 high variance

过拟合曲线会经过所有训练数据点，但是对于新的数据却不一定拟合的好。

**quiz**
![](https://i.loli.net/2020/03/11/XgbPYaKpdJmnF79.png)
**Answer: 第三项**

为什么存在过拟合？ A lot of features with little training data.
解决方法：
1. 减少 feature 数量
    * 手动选择
    * 设计算法自动选择feature（Model selection algorithm后面会讲）
2. 正则化 Regularization
    * 保留所有 feature，但是减少每个 $\theta_j$的 magnitude 值
    * 对于有很多 feature，每一个也都有一点点作用的情况很管用

## 7.2 Cost Function
> penalize 惩罚
> prone  倾向

这一节开始介绍 Regularization 是怎样介绍的，然后讲一下 Regularization 时候的损失函数是什么。

基本思路是在损失函数后面加一个`惩罚项`，以期来降低特定的特征feature的$\theta$的系数，起到使曲线更加平滑的作用。
![](https://i.loli.net/2020/03/11/OZoLUVxzftjcrSX.png)
$$J(\theta) = \frac{1}{2m}[\sum^m_{i=1}(h_{\theta_0}(x^{(i)})-y^{(i)})^2 + \lambda\sum^n_{j=1}\theta_j^2]$$
优化目标 $minJ(\theta)$
**quiz**
![](https://i.loli.net/2020/03/11/IBoYRbVSF6DWdCx.png)
**Answe: 第三项** ，会出现欠拟合了，$\lambda$ 要大但是也不能太大，太大的话会产生太多惩罚，而$\theta_0$的重要性就会大大增加，曲线会逼近于直线。

## 7.3 Regularized Linear Regression
线性回归目前，我们有两种解法，一种是梯度下降，一种是数学公式方法。现在开始介绍正则化线性回归方法。
两种方法都可以正则化。
对于梯度下降法。
梯度迭代公式变为：

![](https://i.loli.net/2020/03/11/QJKcWEiN2lBUPkt.png)

变为
对于公式方法
![](https://i.loli.net/2020/03/11/S95l17VzheGLFjc.png)
## 7.4 Regularized Logistic Regression
对于逻辑回归方法，之前讨论了梯度下降法，也提到了集中高级的优化方法。
损失函数将由：
$$J(\theta) =  -\frac{1}{m}\sum^m_{i=1}[y^{(i)} logh_\theta(x^{(i)})+
(1-y^{(i)})log(1-h_\theta(x^{(i)}))]$$

变为
$$J(\theta) =  -\frac{1}{m}\sum^m_{i=1}[y^{(i)} logh_\theta(x^{(i)})+
(1-y^{(i)})log(1-h_\theta(x^{(i)}))] Z + \frac{\lambda}{2m}\sum^n_{j=1}\theta_j^2$$

其梯度的迭代公式变化和梯度下降的一样。

**quiz**
![](https://i.loli.net/2020/03/11/Jnk1seHYMINWjvd.png)
**Answe:第三项**

最后 Ng 说了，当你学到这的时候你就比硅谷很多赚了很多钱机器学习工程师懂的还多了。哈哈哈
## Test

![](https://i.loli.net/2020/03/11/i4fRGoBL1qlskw7.png)
**Answe:第二项**，第一项如果$\lambda$太大也有可能造成欠拟合，而对于训练集一般是效果会差一点。
![](https://i.loli.net/2020/03/11/Qwfkm1ePutaABvh.png)
**Answe:第二项** $\lambda=1$的时候$\theta_0$的重要性会上升。
![](https://i.loli.net/2020/03/11/vZ2PpBNnOhkqfTw.png)
**Answe:第四项** 过大的$\lambda$会造成欠拟合而不是过拟合
![](https://i.loli.net/2020/03/11/anEj6YVwFGHJ4cP.png)
![](https://i.loli.net/2020/03/11/TdZExSWoQ7Vrn2U.png)
**Answe:第一项** 找过拟合的显然第一个。
![](https://i.loli.net/2020/03/11/2oKeaI3WsxZGjlN.png)
![](https://i.loli.net/2020/03/11/ztUo1cdVCJH9mlK.png)
**Answe:第一项** 找欠拟合的第一个
