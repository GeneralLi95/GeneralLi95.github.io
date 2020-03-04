---
title: Andrew Ng Machine Learning - 5 Logistic Regression
date: 2020-03-03 23:58:30
tags:
  - Andrew Ng Machine Learning
  - MOOC
categories:
  - Machine Learning
  - Stanford Andrew Ng Machine Learning Course
mathjax: True
---
在这一章和上一章中间还有一章是讲 MATLAB 编程的，跳过，不做笔记了，这个没什么好笔记的。

这一章开始讲逻辑回归分析 Logistic Regression。

## 5.1 Classification and Repressentation
### 5.1.1 Classification
从这一节开始讨论分类问题，并介绍一种新的算法——逻辑回归，这也是目前最受欢迎和广泛使用的学习算法之一。

刚开始是二分类(binary classification problem)问题，也即一个 0-1 分类问题，绝了一个现实生活中的例子，肿瘤的良性或恶性，垃圾邮件的是与否。然后首先讲了为什么线性回归不能应用在分类问题。

对于二分类问题，结果是0或者1，而线性回归得出的方程是一条斜线，那么我们必须在斜线上设置一个 threshold，高于它的部分设置为1，低于它的部分设置为0，而这个阈值会受到极端值的非常大影响，导致数据会严重失真，分类器无法准确分类。

**quiz**

Which of the following statements is true?
* If linear regression doesn't work on a classification task as in the previous example shown in the video, applying feature scaling may help.
* If the training set satisfies $0 \le y^{(i)} \le 1$ for every training example
$(x^{(i)}, y^{(i)})$, then linear regression's prediction will also satisfy $0 \le h_\theta(x) \le 1$ for all values of x.
* If there is a feature $x$ that perfectly predicts $y$, i.e. if $y = 1$ when $x \ge c$ and $y=0$ whenever $x<c$ (for some constant $c$), then linear regression will obtain zero classification error.
* **None of the above statements are true.**
答案是第四项，以上都不对。

总结，逻辑回归算法就是对于 y 是固定值场合。

### 5.1.2 Hypothesis Representation
那么用线性回归无法表示分类问题，分类问题的数学表示，开始引入 Sigmoid Function 或 Logistic Function

$$h_\theta(x)=g(\theta^T x)$$
$$z = \theta^Tx$$
$$g(z)=\frac{1}{1+e^{-z}}$$

$g(z)$的函数图像为
![](https://i.loli.net/2020/03/04/4PQK9wnZSRfW85b.png)
完成了从实数域到 0-1 的映射，同时这个函数给出了结果为1的概率，比如 $h_\theta(x)=0.7$ 就意味着分类结果有 70% 的可能性是 1，而结果是 0 的概率即为 30%。
### 5.2.1 Decision Boundary
> recap 记录

决策边界
$h_\theta(x)$ 的值虽然被映射到 0-1 之间，但是它依然不是 0 或者 1，而是0-1之间的连续数，我们需要指定值，当其大于0.5时，认为分类为1，当其小于0.5时，认为分类值为0。**对于$h_\theta(x) = g(\theta^Tx)$其大于或小于0.5，就是 $\theta^Tx$ 大于或小于0。**
![](https://i.loli.net/2020/03/04/s6FvqnYz7gkpjE8.png)

有一些决策边界是非线性边界，比如圆。对于多项式决策方程，其对应的决策边界可能是各种奇奇怪怪的形状。
![](https://i.loli.net/2020/03/04/Prg7bLjCJmElAyR.png)

## 5.2 Logistic Regression Model
### 5.2.1 Cost Function
>convex function 凸函数
>non-convex function 非凸函数
>nonlinear 非线性
>approach to 趋近于

下面开始介绍Logistic Regression 的损失函数

训练集: $\{ (x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),...,(x^{(m)},y^{(m)}) \}$
m个样本
$$x \in \left[ \begin{array}{ccc}x_0 \\\ x_1 \\\ ... \\\ x_n \end{array}\right] x_0=1, y \in \{ 0,1 \}$$

$$h_\theta(x) = \frac{1}{1+e^{-\theta^Tx}}$$
该如何选择参数向量 $\theta$
复习一下线性回归的损失函数:
$$J(\theta) =  \frac{1}{2m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})^2$$
如果我们将这个损失函数直接运用在 Logistic Regression 上，则这个损失函数将是一个非凸函数，而梯度下降法要求损失函数一定要是一个凸函数。
![](https://i.loli.net/2020/03/04/lBKFPu8qoE2WSkj.png)
所以我们需要找到一个损失函数，使其为一个凸函数。

注意这不是分段函数!n
$$
Cost(h_\theta(x),y) = \left\{
\begin{aligned}
-log(h_\theta(x)) \quad if \quad y = 1 \\
-log(1-h_\theta(x)) \quad if \quad y = 0 \\
\end{aligned}
\right.
$$

![](https://i.loli.net/2020/03/04/gdarWysE8BOS6mx.png)
当 y = 1 且 $h_\theta(x)=1$时，这时候损失为0。
而当 $h_\theta(x) \rightarrow 0$ 的时候，$x \downarrow$,$h_\theta(x)\downarrow$，$Cost \uparrow$ 损失函数趋近于无穷大。
而当 y = 0 且 $h_\theta(x)=0$的时候，损失值也是0。
注意 $log(x)$ 和 $log(1-x)$是关于将$log(x)$ 先对称再平移。
当$h_\theta(x) \rightarrow 0$，这时候随着$h_\theta(x) \rightarrow 1$，$x\uparrow$，$h_\theta(x)\uparrow$，$Cost\uparrow$。

![Cost function when y = 0](https://i.loli.net/2020/03/04/rBh4VZHXqWUDiLl.png)

**quiz**

![](https://i.loli.net/2020/03/04/scDfvtC8FA9IYnl.png)
答案是1，2，4.损失函数值是一直大于等于零的。只有当其$y$为 1 或者 0 的时候不是。
### 5.2.2 Simplified Cost Function and Gradient Descent
这节开始尝试简化损失函数的写法。
之前损失函数写法：
$$J(\theta) =  \frac{1}{m}\sum^m_{i=1}Cost(h_\theta(x^{(i)}),y^{(i)})$$
$$
Cost(h_\theta(x),y) = \left\{
\begin{aligned}
-log(h_\theta(x)) \quad if \quad y = 1 \\
-log(1-h_\theta(x)) \quad if \quad y = 0 \\
\end{aligned}
\right.
$$
Note: y = 0 or 1 always

是否存在一种方法，将损失函数写成一行。答案是存在的，因为 y 始终等于 0 或者 1，所以可以构造多
$$
Cost(h_\theta(x),y) =
-y logh_\theta(x)
-(1-y)log(1-h_\theta(x))
$$

所以
$$
J(\theta) =  -\frac{1}{m}\sum^m_{i=1}[y^{(i)} logh_\theta(x^{(i)})+
(1-y^{(i)})log(1-h_\theta(x^{(i)}))]
$$
那么接下来按照梯度下降法，该求偏导数了，`这个非常重要的偏导数求导过程ng居然给省略了...`，可能是如果有这个推导这个课程难度会增加一个 level 吧。

>identical 相同的

### 5.2.3 Advanced Optimization

## 5.3 Muticlass Classification

## Test
