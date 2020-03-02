---
title: Andrew Ng Machine Learning - 4 Linear Regression with Multiple Variables
date: 2020-02-26 17:37:46
tags:
  - Andrew Ng Machine Learning
  - MOOC
categories:
  - Machine Learning
  - Stanford Andrew Ng Machine Learning Course
mathjax: True
---
本节开始讲多元线性回归

## 4.1 Environment Setup Instructions
从这一章开始有了编程作业，所以这一节首先教大家设置编程环境。Andrew Ng 十分推崇 MATLAB 或 Octave，对我非常喜欢的 Python 认为是一种不够灵活的语言，好吧毕竟写 Python 号称要用游标卡尺...

这里推荐了课程软件使用方法，一是以课程链接方式免费使用 MATLAB Online，另外一种是使用开源软件 Octave，语法与 Matlab 一致，但是没有 MATLAB 附带的一系列功能。总之安排是非常贴心的，连软件问题都想到了，真是非常好。
## 4.2 Multivariate Linear Regression
### 4.2.1 Multiple Features
这节开始讨论多元线性回归。多元的同时也意味着有多个特征(features)。同样是从房价预测案例出发，原来的案例里，我们只有一个变量即房屋面积，现在的案例里，增加了「卧室数目」，「层数」，「房屋使用年限」等特征。他们共同决定了房屋的价值。

![](https://i.loli.net/2020/02/28/3O8jcXYtPeRfobn.png)

这里用 $x_i$ 下标，表示某一特征，$x_1$ 为房屋面积，$x_2$ 为卧室数目，而用上标，表示第几个输入案例，如$x^{(i)}$ 表示第 i 个房子。
**quiz:**
![](https://i.loli.net/2020/02/28/KCfO73kcJGFgbPt.png)
**Answer: 第三项**

公式由之前的:
$$h_\theta(x)= \theta_0 + \theta_1 x$$
变成了：
$$h_\theta(x)= \theta_0 + \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 + \theta_4 x_4$$

为了方便表示，也为了矩阵乘法的方便，为了使 $\theta$ 的数目与 $x$ 一样多，我们假设有一个 $x_0 = 1$
这样公式可以写成：
$$h_\theta(x)= \theta_0 x_0 + \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 + \theta_4 x_4$$

这样这个公式可以写成
$$
h_\theta(x) = \left[ \begin{array}{ccc}\theta_0 & \theta_1 & \theta_2 & \theta_3 & \theta_4 \end{array} \right]  \left[ \begin{array}{ccc}x_0 \\\ x_1 \\\ x_2 \\\ x_3 \\\ x_4\end{array}\right]
$$
令
$$ \theta = \left[ \begin{array}{ccc}\theta_0 \\\ \theta_1 \\\ \theta_2 \\\ \theta_3 \\\ \theta_4 \end{array}\right]  x = \left[ \begin{array}{ccc}x_0 \\\ x_1 \\\ x_2 \\\ x_3 \\\ x_4\end{array}\right]$$
也即
$$h_\theta(x) = \theta^T x$$

拓展到 n 维, 则 $\theta$ 与 $x$ 都是 n+1 维向量。

### 4.2.2 Gradient Descent for Multiple Variables
本节讲如何对多元线性回归使用梯度下降法
$$h_\theta(x) = \theta^T  x = \theta_0 x_0 + \theta_1 x_1 + \theta_2 x_2 + ... + \theta_n x_n$$
损失函数为：
$$J(\theta_0,\theta_1,...,\theta_n) =  \frac{1}{2m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})^2$$
即
$$J(\theta) =  \frac{1}{2m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})^2$$




**quiz:**
![](https://i.loli.net/2020/02/28/GY2EqVTRdCwQ9Ax.png)
**Answer: 第一项和第二项**
梯度下降法：
Repeat {
$$\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta_0,\theta_1,...,\theta_n) (j从0开始)$$

}
多元与一元的区别:

![](https://i.loli.net/2020/02/28/J62YncPFH74kv8l.png)
多元每次偏导的时候，每个 $\theta$ 有一个对应的 $x$。

### 4.2.3 Gradient Descent in Pratice I - Feature Scaling
> scaling 缩放
> scale 尺度
> concretely 具体来说

这一部分是特征缩放，目标是使特征的规模(scale)在一个可计算的范围内，或者不同特征的数字值在一个相近的尺度下。

比如 $x_1$ = size(0-2000 $feet^2$)， $x_2$ = number of bedrooms(1-5)，这样得到的$\theta_1$的值会非常小，$\theta_2$会非常大，其等高线图会非常的狭窄。

![](https://i.loli.net/2020/02/28/mN4o8uGbxKz3ReZ.png)

一般来说，特征的缩放，我们尽量使所有的 $x$ 位于 [-1,1] 之间，在同一个数量级的情况下都可以，这样有利于快速收敛。有时需要缩放，有时候也需要扩大。

**Mean normalization**
均值归一化，这样来保证特征均值大约为0。

![](https://i.loli.net/2020/02/28/KgjIENdByH6nbfL.png)

**quiz:**
![](https://i.loli.net/2020/02/28/pWCxaNnhytzGr5U.png)
**Answe: 第四项**

### 4.2.4 Gradient Descent in Pratice II -Learning rate
本节来介绍如何选择一个合适的学习率
随着迭代次数的增加，损失函数的值应该是在不断减小，一般认为如果损失函数的值在一次迭代后下降少于 $10^{-3}$，我们认其收敛。如果损失函数值在增加，那么说明这个损失函数没有正常工作，产生了 `over shot` 说明学习率取的太大了。

quiz 是给出三幅损失函数与迭代次数的图，让选则对应的学习率，比较简单。这一部分还是定性分析了一下学习率大小造成的影响。

### 4.2.4 Features and Polynomial regression
> polynomial regression 多项式回归
> machinery 机器
> frontage 临街

这一部分来告诉我们如何选择特征，以及引入多项式回归的概念，逐渐在线性回归上进行拓展。

![](https://i.loli.net/2020/02/28/PqO6uR4v2QhYN5K.png)
如果进行这一回归的话，Feature scaling 就变得非常重要。

![](https://i.loli.net/2020/02/28/r4HXudmfnCBhFI7.png)
这个例子讲了，如果担心平方函数后面会下降的话，可以采用根方函数以获得更好的拟合

**quiz**
![](https://i.loli.net/2020/02/28/qSsO97zK1Z8wXHF.png)
**Answer: 第三项**
这个小习题是考察在这种情况下如何进行 feature scaling.

## 4.3 Computing Parameters Analytically
### 4.3.1 Normal Equation
>Analytically 分析地

这里讲了计算线性回归参数 $\theta$ 的一种一步到位的数学方法，而不是用梯度下降法，是直接用数学公式得出参数，高中数学内容似乎有最小二乘法来计算这一说。这里需要注意的是，在 n 较小的时候(几百数量级)利用这种方法是可以的，但是在 n 达到 thousand 的时候，计算机处理矩阵计算会变得非常慢，所以便不适合。

一种方法是，对损失函数求偏导，令其等于0，然后解方程。这其实就是梯度下降法的小小 trick。
![](https://i.loli.net/2020/02/28/dcJZz6PNU7LbvFu.png)

另外一种就是 直接一个公式(公式推导略)
$$\theta = (X^TX)^{-1} X^Ty$$

![](https://i.loli.net/2020/03/02/O1XRPxLVtaN8nCy.png)

在 Octave里编程
```matlab
pinv (X'*X)*X'*y
```
对比梯度下降法和公式法，梯度下降法需要选择一个学习率 $\alpha$，需要许多迭代次数，对于公式法，不需要选择 $\alpha$ 也不需要迭代次数，但是梯度下降法对于 n 很大的场景也适用，而公式法在n很大的情况下，其中的求逆操作需要的计算复杂度是 $O(n^3)$，所以在 n 达到 thousand 级别时，便不能再使用公式法。
### 4.3.2 Normal Equation Noninvertibility
> Noninvertibility 不可逆性

由于公式法中涉及一个矩阵求逆操作，所以这一节讲的是如果该矩阵不可逆的情况下如何。实际上这涉及一个广义逆矩阵，对于不可逆矩阵，是存在广义逆矩阵分解的用 `pinv` 就可以计算广义逆矩阵。对于不可逆的情况，有两种解释
* 存在冗余的特征，在数学上的解释就是线性相关
* 特征太多，去掉一些特征或者使用正则化 regularization 会解决

## Test
手敲这部分设计太多公式表格实在是太慢了，所以这次开始用截图代替。

![](https://i.loli.net/2020/03/02/7cnFJrEbIMNmdAT.png)
**Answer: -0.47**
这个题考察哦的实际上就是 feature scaling。用公式:
$$\frac{x-\mu}{max-min}$$
![](https://i.loli.net/2020/03/02/z7eBrdiPF2hyLEW.png)
**Answer: 第二项**
考察学习率调整的概念
![](https://i.loli.net/2020/03/02/ylQXT62ceIthGL1.png)
**Answer: 第四项**
考察对于基本推导的理解
![](https://i.loli.net/2020/03/02/ukhnOR5vfeQCNdb.png)
**Answer: 第四项**
考察梯度下降法和公式方法的使用场景。
![](https://i.loli.net/2020/03/02/PFORhwg2avIUeGf.png)
**Answer: 第四项**
考察 feature scaling的作用和原理。
