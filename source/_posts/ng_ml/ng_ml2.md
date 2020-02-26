---
title: Andrew Ng Machine Learning - 2 Linear regression with one variable
date: 2020-01-10 19:33:51
tags:
    - Andrew Ng Machine Learning
    - MOOC
categories:
    - Machine Learning
    - Stanford Andrew Ng Machine Learning Course
mathjax: true
---
本节开始以 Linear regression with one variable(一元线性回归)为例，讲述机器学习的基本概念。包括 Model （模型）， Cost Function （损失函数），Parameter Learning （参数学习）等。

## 2.1 Model Representation
> hypothesis 假设
> terminology 术语

以房价预测 Housing Prices 开始，属于监督学习，Regression
![](https://i.loli.net/2020/02/21/F7ouwtfCk5Tjiga.png)
![](https://i.loli.net/2020/02/21/xqal1GmMAQsEiX2.png)
这一小节主要就讲了我们如何表示一个模型，输入，输出，函数等一些概念。

## 2.2 Cost function
### 2.2.1 Definetion of the cost function
假设：
$$h_{\theta}(x) = \theta_0+\theta_1x$$
$\theta_0$ 和 $\theta_1$ 即为这个模型的参数，也即需要学习得出的量。
在一元线性回归中，我们希望找出 $\theta_0$ 和 $\theta_1$，使得拟合的曲线非常适合数据。那么问题可以转坏为一个求最小化的问题，即求
$$min_{\theta_0,\theta_1}(h_\theta(x)-y)^2$$
取最小值时的 $\theta_0$ 和 $\theta_1$。

$$min_{\theta_0,\theta_1} \frac{1}{2m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})^2$$

所以我们列出 Cost function ，损失函数
$$J(\theta_0,\theta_1) =  \frac{1}{2m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})^2$$
我们所求即
$$min_{\theta_0,\theta_1}J(\theta_0,\theta_1)$$
这种损失函数也称为 Square error function，平方误差函数。对于大多数Regression问题中，Square error function works well。
### 2.2.2 Why we use cost function
首先做了一个假设，假设$\theta_0=0$,来简化问题。
![](https://i.loli.net/2020/02/21/VAWfwnGziKTjRxl.png)
$h_\theta(x)$是对于固定的 $\theta$ ，是一个$x$的函数，$J(\theta_1)$是一个关于$\theta$的函数。
**quiz:**
![](https://i.loli.net/2020/02/21/AK4CsdXQpN3HG7x.png)
注意 $J(\theta_1)$ 是关于 $\theta$的函数，所以$J(0)$，对应的$h(x)$，就是x轴，所以结果是 $(1+2^2+3^2)/6=14/6$
![](https://i.loli.net/2020/02/21/xfBJWAMpEPUONsT.png)
这张图表示了$J(\theta_1)$变化曲线，可见$\theta_1=1$的时候$J(\theta_1)=0$，完美拟合。

### 2.2.3 Deeper and better intuition about cost function
> intuition 直觉
> contour plot 等高线

上一小节讨论了简化的损失函数，即在$\theta_0=0$的情况下，这一节开始讨论包含 $\theta_0$的情况下的损失函数。当只有一个参数($\theta_1$)的时候，损失函数$J(\theta)$的曲线是一个二次函数。而当有两个参数的时候，损失函数的表式编程了一个三维曲面。而将三维曲面二维化的方式就是等高线`contour plot`。在具有更多参数的时候，损失函数将会是高维曲面，无法直观看出来。
![](https://i.loli.net/2020/02/21/b6Qw5e4kxALZ2Mf.png)
![](https://i.loli.net/2020/02/21/EqnX5URdZVHK4bN.png)

## 2.3 Parameter Learning
### 2.3.1 Gradient Descent
> optimum 最佳
> dirivative 偏导

损失函数如何求取最佳的 $\theta$组合，通过图像绘制是一种方法，但是高维损失函数无法直接绘出。所以需要有自动化的方法。本节介绍一种方法，Gradient Descent 梯度下降法。这是在深度学习中广泛使用的函数最优化方法，并非只用在线性回归的损失函数上。
在已知$J(\theta_0,\theta_1)$，也可能是$J(\theta_0,\theta_1,...,\theta_n)$，的情况下求$minJ(\theta_0,\theta_1)$

Outline：
* Start with some $\theta_0, \theta_1$
* Keep changing $\theta_0,\theta_1$ to reduce $J(\theta_0,\theta_1)$ until we hopefully end up at a minimum

在三维曲面上，损失函数的曲线，像地形，寻找最小值的过程像一个下山的过程，梯度下降法就是寻找最快下山路径的算法。这里存在一些问题，地形可能存在，「鞍点」local optimum，即局部最佳值，选取不同的初始值，可能会走入不同的局部最佳值。

![](https://i.loli.net/2020/02/21/V6fa3DBHTCymUFK.png)

> Gradient descent algorithm
> repeat until convergence
$\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_0} J(\theta_0,\theta_1)$ （for $j=0$ and $j=1$)

$\alpha $在此处即为 learning rate，学习率，后面会讲到。
![](https://i.loli.net/2020/02/21/RuQOv6yzEbZaiMD.png)

注意，要两个值都计算完了再更新，而不是计算一个更新一个。

> := 是在 Pascal 语言当中作为 赋值语句用的
> simultaneous 同时的，并发的

**quiz**
![](https://i.loli.net/2020/02/21/7kQpWXAIU6GgouN.png)

刚开始还以为这道题涉及求导，后来发现这道题根本不需要求导，直接带入就出来了。选第二项。

### 2.3.2 Gradient Descent Intuition
> Intuition 直觉

前一部分讲了梯度下降法的定义，这一部分进一步讲了其原理。
![](https://i.loli.net/2020/02/21/QOy5aolrA168DP3.png)

一步步拆解了梯度下降法的一个原理，在曲线右侧，斜率为正，更新后$\theta_1$减小，向左移动，在曲线左侧，斜率为负，更新后负负得正,$\theta_1$增加，向右移动。而每次移动多少，除了受斜率影响，还会受系数 $\alpha$ 影响，做个 $\alpha$，即成为学习率。显然，如果学习率太小，则每次移动速度会非常小，收敛速度很慢，如果学习率很大，则收敛速度会过快，甚至`over shoot`

**quiz**
![](https://i.loli.net/2020/02/21/mEBtPCvz54Gh1eQ.png)
* **Leave $\theta_1$ unchanged**
* Change $\theta_1$ in a random direction
* Move $\theta_1$ in the direction of the global minimum of $J(\theta_1)$
* Derease $\theta_1$
按照公式此处$\theta_1$
按照公式此处$\theta_1$偏导数是0，所以不会变。这也显示了它可能会陷入局部最优解。

先暂时不考虑局部最优解问题，由于每次更新后，偏导数（即斜率）是越来越小的，所以梯度下降法的 steps，会随之变小，即使学习率$\alpha$是固定的。

### 2.3.2 Gradient For Linear regression
> convex funciton 凸函数，关于凸函数，高中数学的定义讲的十分混乱，有非常多的版本，这里我们采用和课程一致的，也是维基百科的版本，即二阶导数大于零，bowl shape function 碗型函数

前一部分都是定性的讲解梯度下降法的原理，这一部分开始带入一元线性回归的损失函数，来实际使用一下梯度下降法。
![](https://i.loli.net/2020/02/21/pRO9rDBC85zqA2W.png)
前文已经提到，一元线性回归的损失函数为

$$J(\theta_0,\theta_1) =  \frac{1}{2m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})^2$$
将$h_{\theta}(x) = \theta_0 + \theta_1 x$ 代入，可得
$$J(\theta_0,\theta_1) =  \frac{1}{2m}\sum^m_{i=1}(\theta_0 + \theta_1 x^{(i)}-y^{(i)})^2$$
分别对 $\theta_0$ 和 $\theta_1$求偏导可得

$$\frac{\partial}{\partial_{\theta_0}}J(\theta_0,\theta_1) =  \frac{1}{m}\sum^m_{i=1}(\theta_0 + \theta_1 x^{(i)}-y^{(i)})$$
$$\frac{\partial}{\partial_{\theta_1}}J(\theta_0,\theta_1) =  \frac{1}{m}\sum^m_{i=1}(\theta_0 + \theta_1 x^{(i)}-y^{(i)})x^{(i)}$$
即如下图所示的推导
![](https://i.loli.net/2020/02/21/aIeTfuiDLPkAUM5.png)
继而，迭代公式即

$$\theta_0 :=\theta_0 - \alpha \frac{1}{m}\sum^m_{i=1}(\theta_0 + \theta_1 x^{(i)}-y^{(i)})$$

$$\theta_1 := \theta_1 - \alpha \frac{1}{m}\sum^m_{i=1}(\theta_0 + \theta_1 x^{(i)}-y^{(i)})x^{(i)}$$

暂时不考虑局部最优解问题，因为线性回归问题的损失函数都是`convex functinon`凸函数，也即碗型函数。这些函数没有 `local optimum`，只有一个 `global optimum`。

'Batch' 批处理，梯度下降的每次迭代都要用到所有训练数据，因为计算损失函数的时候需要代入所有的 x,y 值。在线性回归问题中，batch 是所有数据，在其他一些机器学习问题中，batch 有时候是整个训练集的子集。

**quiz:**

Which of the following are true statements? Select all that apply.
* To make gradient descent converge, we must slowly decrease $\alpha$ over time.
* Gradient descent is guaranteed to find the global minimum for any function $J(\theta_0,\theta_1)$
* **Gradient descent can converge even if $\alpha$ is kept fixed. (But $\alpha$ cannot be too large, or else it may fail to converge.)**
* **For the specific choice of cost function $J(\theta_0,\theta_1)$ used in linear regression, there are no local optima (other than the global optimum).**

这道题让选正确表述，前面都已经写得比较细了，一个是即使学习率固定，梯度下降法的步长依然会依次减小。另外一个是对于现行回归问题，没有局部最优，只有全局最优，所以不需要考虑「鞍点」的问题。

## 2.4 Test
1.Consider the problem of predicting how well a student does in her second year of college/university, given how well she did in her first year.
Specifically, let x be equal to the number of "A" grades (including A-. A and A+ grades) that a student receives in their first year of college (freshmen year). We would like to predict the value of y, which we define as the number of "A" grades they get in their second year (sophomore year).
Refer to the following training set of a small sample of different students' performances (note that this training set may also be referenced in other questions in this quiz). Here each row is one training example. Recall that in linear regression, our hypothesis is $h_\theta(x) = \theta_0 + \theta_1x$, and we use mm to denote the number of training examples.

x|y
---|---
3|4
2|1
4|3
0|1

For the training set given above, what is the value of mm? In the box below, please enter your answer (which should be a number between 0 and 10).
**Answer:4**
这一题求 m，其实就是找有几个样本，不需要计算直接得到4.
2.Consider the following training set of $m = 4$ training examples:

x|y
---|---
1|0.5
2|1
4|2
0|0

Consider the linear regression model $h_\theta(x) = \theta_0 + \theta_1x$. What are the values of $\theta_0$ and $\theta_1$ that you would expect to obtain upon running gradient descent on this model? (Linear regression will be able to fit this data perfectly.)

* **$\theta_0 = 0, \theta_1 = 0.5$**
* $\theta_0 = 1, \theta_1 = 0.5$
* $\theta_0 = 0.5, \theta_1 = 0.5$
* $\theta_0 = 1, \theta_1 = 1$
* $\theta_0 = 0.5, \theta_1 = 0.5$

这道题在坐标图上一画就很直观。
3.Suppose we set $\theta_0 = -1, \theta_1 = 2$ in the linear regression hypothesis from Q1. What is $h_{\theta}(6)$?
    **Answer: 11**
$2 \times 6-1 =11$

4.In the given figure, the cost function $J(\theta_0,\theta_1)$ has been plotted against $\theta_0$ and $theta_1$, as shown in 'Plot 2'. The contour plot for the same cost function is given in 'Plot 1'. Based on the figure, choose the correct options (check all that apply).
![](https://i.loli.net/2020/02/26/eOTamsXcpL5FJdv.png)

* Point P (The global minimum of plot 2) corresponds to point C of Plot 1.
* **If we start from point B, gradient descent with a well-chosen learning rate will eventually help us reach at or near point A, as the value of cost function $J(\theta_0,\theta_1)$ is minimum at A.**
* If we start from point B, gradient descent with a well-chosen learning rate will eventually help us reach at or near point A, as the value of cost function $J(\theta_0,\theta_1)$ is maximum at point A.
* **Point P (the global minimum of plot 2) corresponds to point A of Plot 1.**
* If we start from point B, gradient descent with a well-chosen learning rate will eventually help us reach at or near point C, as the value of cost function $J(\theta_0,\theta_1)$ is minimum at point C.

这道题考察对于损失函数的理解。

5.Suppose that for some linear regression problem (say, predicting housing prices as in the lecture), we have some training set, and for our training set we managed to find some $\theta_0$, $\theta_1$ such that $J(\theta_0, \theta_1)=0$.
Which of the statements below must then be true? (Check all that apply.)
* For this to be true, we must have $\theta_0 = 0$ and $\theta_1 = 0$ so that $h_\theta(x) = 0$
* This is not possible: By the definition of $J(\theta_0, \theta_1)$, it is not possible for there to exist $\theta_0$ and $\theta_1$ so that $J(\theta_0, \theta_1) = 0$
* **For these values of $\theta_0$ and $\theta_1$ that satisfy $J(\theta_0, \theta_1) = 0$, we have that $h_\theta(x^{(i)}) = y^{(i)}$ for every training example $(x^{(i)}, y^{(i)})$**
* We can perfectly predict the value of yy even for new examples that we have not yet seen.(e.g., we can perfectly predict prices of even new houses that we have not yet seen.)
* Gradient descent is likely to get stuck at a local minimum and fail to find the global minimum.
* For this to be true, we must have $y^{(i)} = 0$ for every value of $i = 1, 2, \ldots, m$.
* For this to be true, we must have $\theta_0 = 0$ and $\theta_1 = 0$ so that $h_\theta(x) = 0$
* **Our training set can be fit perfectly by a straight line, i.e., all of our training examples lie perfectly on some straight line.**

这道题损失函数是零，说明，所有点都与拟合线重合，对于一元回归问题，一定是直线。
