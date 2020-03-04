---
title: Machine Learning 编程作业 1 Linear Regression
date: 2020-03-02 19:09:51
tags:
  - Andrew Ng Machine Learning
  - MATLAB
categories:
  - Machine Learning
  - Stanford Andrew Ng Machine Learning Course
mathjax: True
---
这一部分是 Andrew Ng Machine Learning 课程的第一次编程作业，位于课程第二周。编程内容为 Linear Regression。

第一周作业包含文件:
```
ex1.m - Octave/MATLAB script that steps you through the exercise
ex1 multi.m - Octave/MATLAB script for the later parts of the exercise
ex1data1.txt - Dataset for linear regression with one variable
ex1data2.txt - Dataset for linear regression with multiple variables
submit.m - Submission script that sends your solutions to our servers
[*] warmUpExercise.m - Simple example function in Octave/MATLAB
[*] plotData.m - Function to display the dataset
[*] computeCost.m - Function to compute the cost of linear regression
[*] gradientDescent.m - Function to run gradient descent
[†] computeCostMulti.m - Cost function for multiple variables
[†] gradientDescentMulti.m - Gradient descent for multiple variables
[†] featureNormalize.m - Function to normalize features
[†] normalEqn.m - Function to compute the normal equations
```
* 为需要完成的部分，† 为选做内容。

其中 `ex1.m` 和 `ex1_multi.m` 设置了读取数据和调用函数的代码，所以我们只需要根据要求写最核心的代码即可。

## 1. Simple Octave / MATLAB function
这是最简单的一个，主要是尝试一下，跟一般编程的 hello world 一样，在 `warmExercise.m` 书一个

```matlab
A = eye(5);
```
即可，输入后在 Command Window 会输出一个 5x5 的单位矩阵。然后在 Command Window 输入 submit，实际上相当于执行 `submit.m`，然后输入课程账号的邮箱和 课程生成 Token 密钥即可，提交，流程上还是非常方便的。

## 2. Linear regression with one variable
下面开始编写一元线性回归，课程方还是给了一个背景，要预测一个餐厅的一车食物的利润。其中数据存放在 `ex1data1.txt` 里，是许多航数据，没行以逗号分隔两个数，显然一列是 x 一列是 y 咯。关于读数据的操作存放在 `ex1.m`里，不需要我们修改。研究一下代码，其中读数据的部分是

```matlab
data = load('ex1data1.txt');
X = data(:, 1); y = data(:, 2);
m = length(y); % number of training examples
```
### 2.1 Plotting the Data
做一下数据可视化，在正式计算前，做一下数据可视化可以帮助理解数据。一个 `plotData.m` 脚本需要我们来填充，但是在文档里已经给出了代码，直接填上。

```matlab
plot(x, y, 'rx', 'MarkerSize', 10); % Plot the data
ylabel('Profit in $10,000s'); % Set the y-axis label
xlabel('Population of City in 10,000s'); % Set the x-axis label
```
其中 `rx` 是指散点为红色的 `x` 点
然后执行 `ex1.m` 得到如下图所示

![](https://i.loli.net/2020/03/03/r8ZeN1akbQTh6Wv.png)
### 2.2 Gradient Descent
开始写梯度下降法的代码
#### 2.2.1 Update Equations
首先根据损失函数的公式
$$J(\theta_0,\theta_1) =  \frac{1}{2m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})^2$$
其中
$$h_{\theta}(x) =\theta ^T x= \theta_0+\theta_1x$$
求偏导之后，得到迭代公式
$$\theta_0 := \theta_0 - \alpha \frac{1}{m}\sum^m_{i=1}(\theta_0 + \theta_1 x^{(i)}-y^{(i)})$$
$$\theta_1 := \theta_1 - \alpha \frac{1}{m}\sum^m_{i=1}(\theta_0 + \theta_1 x^{(i)}-y^{(i)})x^{(i)}$$

#### 2.2.2 Implementation
在 `ex1.m` 里面，已经将线性回归用到的数据读入了，现在需要在程序中体现$\theta$，需要增加一维数据，编程不可能设未知量，必须指定初始值，这也是梯度下降法的要求。这一部分代码也是给出的

```matlab
X = [ones(m, 1), data(:,1)]; % Add a column of ones to x
theta = zeros(2, 1); % initialize fitting parameters

% Some gradient descent settings
iterations = 1500;
alpha = 0.01;
```
注意此处将 X 变量拓展为二维其中一维即为 $x_0$，$x_0$是为了凑向量和 $\theta_0$相乘的，默认是1 是,迭代次数设置为1500次，学习率设置为0.01。

#### 2.2.3 Computing the cost $J(\theta)$
计算损失函数 $J(\theta)$
这是一个需要自己写的代码了，完善 `computeCost.m`，前面公式已经给出了，所以需要写代码将公式翻译出来。
```matlab
for i = 1 : m
    J = J + (theta(1) + theta(2)*X(i,2) - y(i))^2;
    end

J = J /(2*m);
```
用向量的方式来写这个，不需要使用 for 循环即直接完成。用到的 `.^` 为向量的平方，即将所有元素乘方。
```
J = sum( (X * theta - y).^2) / (2 * m);
```
注意 MATLAB 的数组是从首位下标是1，不是0，写习惯了Python，真不习惯 MATLAB，首次运行的结果应该是 32.07，写到这一步，我们即可再提交一次。

#### 2.2.4 Gradient descent
梯度下降，计算完损失函数之后需要更新 $\theta$，下面开始写更新的代码，更新的代码位于 `gradientDescent.m`里，也是翻译公式。

```matlab
sum1 = 0;
sum2 = 0;
for i = 1:m
    sum1 = sum1 + theta(1) + theta(2)*X(i,2) - y(i);
    sum2 = sum2 + (theta(1) + theta(2)*X(i,2) - y(i))*X(i,2);
end
theta(1) = theta(1) - alpha / m * sum1;
theta(2) = theta(2) - alpha / m * sum2;

```
运行 `ex1.m`得到下图。

在这种写法之外，其实刚才的乘法还可以通过一种向量计算来完成即

```matlab
H = X * theta
   T = [0 ; 0];
   for i = 1 : m,
      T = T + (H(i) - y(i)) * X(i, :)';
   end

   theta = theta - alpha * T / m;
```

![](https://i.loli.net/2020/03/03/6I4LpBaO1ocZQzC.png)

再次 submit，至此，这次的编程作业即完成。后面的内容皆为选做。
