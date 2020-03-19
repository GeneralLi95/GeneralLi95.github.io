---
title: Machine Learning 编程作业 2 Logistic Regression
date: 2020-03-12 11:22:48
tags:
  - Andrew Ng Machine Learning
  - MATLAB
categories:
  - Machine Learning
  - Stanford Andrew Ng Machine Learning Course
mathjax: True
---
开始第二次编程作业，对应课程内容第三周，第六章，逻辑回归，第七章，正则化。本编程作业中没我们将在两个不同的数据集上实现逻辑回归。

第二周作业包含文件:
```
ex2.m - Octave/MATLAB script that steps you through the exercise
ex2 reg.m - Octave/MATLAB script for the later parts of the exercise
ex2data1.txt - Training set for the ﬁrst half of the
exercise ex2data2.txt - Training set for the second half of the exercise
submit.m - Submission script that sends your solutions to our servers
mapFeature.m - Function to generate polynomial features
plotDecisionBoundary.m - Function to plot classiﬁer’s decision boundary
[*] plotData.m - Function to plot 2D classiﬁcation data
[*] sigmoid.m - Sigmoid Function
[*] costFunction.m - Logistic Regression Cost Function
[*] predict.m - Logistic Regression Prediction Function
[*] costFunctionReg.m - Regularized Logistic Regression Cost
```
其中任务分为两部分， `ex2data1.txt` 是前半部分用到的数据，对应第六章，逻辑回归。`ex2data2.txt` 是后半部分用到的数据，对应第七章，正则化（带惩罚项的逻辑回归）。`mapFeature.m` 是产生多项式特征大函数，我们需要完成的是`plotData.m`绘制 2D 分类数据，盲猜可能是个散点图。`sigmoid.m` 编写 sigmoid 函数，这是构造损失函数的必备，以及逻辑回归的损失函数，预测，以及正则化的逻辑回归损失函数。

## 1 Logistic Regression
这部分联系，需要建立一个逻辑回归模型去预测一个学生能否被某个大学录取。训练数据集是 学生们在两个考试中的成绩。
### 1.1 Visualizing the data
一般在实现学习算法前，可视化数据统称会有一个直观的理解。需要完成一个练习 `plotData.m`，不过这个练习时不给分的，所以课程辅导资料里面给出了代码。

```matlab
% Find Indices of Positive and Negative Examples
pos = find(y==1); neg = find(y==0);

% Plot Examples
plot(X(pos, 1), X(pos, 2), 'k+', 'LineWidth', 2, 'MarkerSize', 7);

plot(X(neg, 1), X(neg, 2), 'ko', 'MarkerFaceColor', 'y', 'MarkerSize', 7);

```
每个 pos 有两个变量，分别是横纵坐标，X(pos, 1) 就是横坐标数组，`k+` 意思是用个小加号表示点。 `ko` 就是用个小圈表示一个点。

在 MATLAB 语言中 `disp()` 相当于 python 中的 `print()`  可以用来打印输出，调试。
### 1.2 Implementation
接下来开始实施逻辑回归
#### 1.2.1 warmUpExercise: sigmoid function
首先是一个热身小编程，实现 sigmoid function, 公式是：

$$g(z) = \frac{1}{1+e^{-z}}$$

直接翻译公式就好了，非常简单，在 `sigmod.m` 中指定位置

```matlab
g = 1 / (1 + e^(-z));
```

一行代码搞定，然后可以在 Command line 输入 sigmoid(0) 进行测试，输出结果应该是 0.5。然后可以提交一次。当然这样提交是通过不了的，因为 z 可能是向量或矩阵(vectors and matrices) 所以，对于矩阵 1 应该表示单位矩阵。起初我尝试用 eye(size(z)) 代替1 后来发现用  `./` 代替 `/`, `.*` 代替`*`

```matlab
g = 1 ./ (1 + e.^(-z));
```
#### 1.2.2 Cost function and gradient
接下来开始实行 损失函数。公式是

$$
J(\theta) =  -\frac{1}{m}\sum^m_{i=1}[y^{(i)} logh_\theta(x^{(i)})+
(1-y^{(i)})log(1-h_\theta(x^{(i)}))]
$$

$$\frac{\partial}{\partial \theta}J(\theta)=
\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)}) - y^{(i)})x^{(i)}_j
$$

在第一章的练习里我们已经知道，求和可以用 sum 函数和向量计算，代替用 for 循环和繁杂的计算，这次学聪明了，不再写循环了，直接用向量计算。

```matlab
J = - sum( y .* log(sigmoid(X * theta)) + (1 - y).*log(1 - sigmoid(X * theta ))) ./ m;

grad = sum(((sigmoid(X * theta) - y)) .* X) ./ m;
```

#### 1.2.3 Learning parameters using fminunc

基础搭建好了，开始真正的学习过程。在 Logistic Regression 中，我们不再像线性回归中一样去手动迭代，而是使用 matlab 一个内建函数 `fminunc` 。`fminuc` 是一个优化处理器(optimization solver) 来找一个非限制函数(unconstranined)的最小值。在 Logistic Regression 中因为 $\theta$ 可以取任何值，所以该函数也是一个非限制函数。

`fminuc` 的输入分两部分
* 要优化的参数的初始值，即给定初值。
* 一个计算损失函数和梯度的函数。

官方给出了调用改函数的代码，在`ex2.m`里：

```matlab
%  Set options for fminunc
options = optimset('GradObj', 'on', 'MaxIter', 400);

%  Run fminunc to obtain the optimal theta
%  This function will return theta and the cost
[theta, cost] = ...
	fminunc(@(t)(costFunction(t, X, y)), initial_theta, options);
```

这个代码中 `GradObj` `on` 的意思是我们的函数既返回 损失值，也返回梯度。同时设置了最大迭代次数是 400。

为了具体指出我们要优化的函数，后面的代码 `@(t)` 是一个 'short-hand'，这创建了一个函数，参数 t 是我们的损失函数。所以我们只需要提供一个计算损失和梯度的函数即可。而这部分代码我们在上一部分其实已经写过了，所以我们可以直接运行 `ex2.m` 来看结果。

#### 1.2.4 Evaluating logistic regression
下面即是应用模型的时候了，可以根绝一个学生的两科成绩来看看他到底会不会被录取了。

应用模型就是把学习到的参数带入最初的方程即可

```
p = sigmoid(X * theta);
```
但是注意这时候还是不行的， p 是连续值，而逻辑回归的结果只有0 和1 ，所以：

```
p = sigmoid(X * theta) >= 0.5;
```

## 2 Regularized logistic regression
这一部分对应第七章，正则化，课程内容中给出了线性回归核逻辑回归的正则化，编程作业只需要完成逻辑回归的正则化。

首先，开始还是可视化数据，这次的 散点图分布与之前不同，是大概是一个圆，圆内是一类，圆外是一类，那么分割曲线必定是一个多项式，而不是一条直线了。

### 2.2 Feature mapping
官方提供了一个函数  `mapFeature.m` ，可以根据 x1,x2 生成最高六次项的特征。 那么这将是一个 28 维的向量 $（1,x_1,x_2,x_1^2, x_1 x_2, x_2^2, ..., x_2^6)^T$ 一共有（1+2+3+4+5+6+7=28）维。这样很有可能会出现过拟合问题，而实际上圆的话 只需要最高到二次型就可以了。

### 2.3 Cost function and gradient
对于正则化的逻辑回归，我们实际上是增加一个惩罚变量，来减少高次项的影响。其损失函数为

$$J(\theta) =  -\frac{1}{m}\sum^m_{i=1}[y^{(i)} logh_\theta(x^{(i)})+
(1-y^{(i)})log(1-h_\theta(x^{(i)}))] + \frac{\lambda}{2m}\sum^n_{j=1}\theta_j^2$$

其梯度为
$$\frac{\partial}{\partial \theta_0}J(\theta)=
\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)}) - y^{(i)})x^{(i)}_j  ~~~~ (j=0)
$$

$$\frac{\partial}{\partial \theta_j}J(\theta)=
\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)}) - y^{(i)})x^{(i)}_j + \frac{\lambda}{m} \theta_j~~~~~~ (j>=1)
$$

我们需要在 `FunctionReg` 里面完成这个公式。

首先是 Cost function

```matlab
J = - sum( y .* log(sigmoid(X * theta)) + (1 - y).*log(1 - sigmoid(X * theta ))) ./ m + lambda .* sum(theta .* 2) ./ (2 * m);
```

为了美观，我们将其写成

```matlab
H = sigmoid(X* theta)
T = y.*log(H) + (1-y).*log(1-H)
J = - sum(T) ./ m +lambda/(2*m) .* sum(theta.*2);

```

对于梯度 grad ，因为它涉及两种情况，所以直接加是不行的，这时候必须要要写循环分开处理了。还应该将 $\theta$ 改造。将其第一项替换为0即。
```matlab
for i = 1 : m,
	grad = grad + (H(i) - y(i)) * X(i,:)';
end

ta = [0; theta(2:end)];

grad = 1/m*grad + lambda/m*ta;
```

#### 2.3.1 Learning parameters using fminunc
与上一张类似 使用 `fminunc` 内建函数来找最小值。不需要我们单独写程序了。

### 2.4 Plotting the decision boundary
绘制决策线，发现基本是一个近似圆，后面的部分皆为选做。
