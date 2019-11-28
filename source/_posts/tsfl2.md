---
title: TensorBoard使用示例
date: 2018-07-25 11:28:01
tags:
  - 机器学习
  - TensorFlow
  - TensorBoard
categories: 机器学习
catalog: true
---

为了更方便TensorFlow程序的理解、调试与优化，Google发布了一套叫做TensorBoard的可视化工具，可以用TensorBoard来展现TensorFlow的图像，绘制图像生成的定量指标图以及附加数据。

TensorBoard设置完成之后的样子应该如下图：
![](http://wiki.jikexueyuan.com/project/tensorflow-zh/images/mnist_tensorboard.png)

其基本原理见[TensorBoard中文手册](http://wiki.jikexueyuan.com/project/tensorflow-zh/how_tos/summaries_and_tensorboard.html)，内有详细的介绍。

本文参考了[放羊的水瓶的博文](https://www.cnblogs.com/fydeblog/p/7429344.html)。

下面通过三个例程，来讲解其使用：
## 例程1  矩阵相乘 tfboard1.py

```python
import tensorflow as tf
with tf.name_scope('graph') as scope:
    matrix1 = tf.constant([[3., 3.]], name = 'matrix')   # 一行两列
    matrix2 = tf.constant([[2.], [2.]], name = 'matrix2') # 两行一列
    product = tf.matmul(matrix1, matrix2, name = 'product')


sess = tf.Session()

writer = tf.summary.FileWriter("logs1/", sess.graph)

init = tf.global_variables_initializer()

sess.run(init)
```

tf.name_scope函数是作用域名，上述代码斯即在graph作用域op下，又有三个op（分别是matrix1，matrix2，product),用tf函数内部的name参数命名，这样会在tensorboard中显示。

运行上述代码后，在项目所在目录会生成"logs1"目录（可以自定义名字），然后在命令行运行：

```shell
tensorboard --logdir logs1
```
即可在本机6006端口调用TensorBoard。可以通过浏览器打开使用。

## 例程2 线性拟合（一） tfboard2.py
例程1中没有任何训练过程，比较简单，下面通过这个例子来画出它的张量流动图。

```python

import tensorflow as tf
import numpy as np

# 准备原始数据
with tf.name_scope('data'):
    x_data = np.random.rand(100).astype(np.float32)
    y_data = 0.3*x_data + 0.1

# 参数设置
with tf.name_scope('parameters'):
    weight = tf.Variable(tf.random_uniform([1], -1.0, 1.0))
    bias = tf.Variable(tf.zeros([1]))

# 得到 y_prediction
with tf.name_scope('y_prediction'):
    y_prediction = weight*x_data + bias

# 计算损失率compute the loss
with tf.name_scope('loss'):
    loss = tf.reduce_mean(tf.square(y_data - y_prediction))

#
optimizer = tf.train.GradientDescentOptimizer(0.5)

with tf.name_scope('train'):
    train = optimizer.minimize(loss)

with tf.name_scope('init'):
    init = tf.global_variables_initializer()

sess = tf.Session()
writer = tf.summary.FileWriter("logs2/",sess.graph)
sess.run(init)

for step in range(101):
    sess.run(train)
    if step%10 == 0:
        print(step, 'weight', sess.run(weight), 'bias:', sess.run(bias))

```

## 例程3 线性拟合（二） tfboard3.py

对例程二代码进行修改，尝试tensorboard的其他功能，例如scalars,distributions,histograms,这些功能对于分析学习算法的性能有很大帮助。


```python
import tensorflow as tf
import numpy as np

with tf.name_scope('data'):
    x_data = np.random.rand(100).astype(np.float32)
    y_data = 0.3*x_data + 0.1

with tf.name_scope('paremeters'):
    with tf.name_scope('weights'):
        weight = tf.Variable(tf.random_uniform([1], -1.0, 1.0))
        tf.summary.histogram('weight', weight)
    with tf.name_scope('biases'):
        bias = tf.Variable(tf.zeros([1]))
        tf.summary.histogram('bias', bias)

with tf.name_scope('y_prediction'):
    y_prediction = weight*x_data + bias

with tf.name_scope('loss'):
    loss = tf.reduce_mean(tf.square(y_data - y_prediction))
    tf.summary.scalar('loss', loss)

optimizer = tf.train.GradientDescentOptimizer(0.5)

with tf.name_scope('train'):
    train = optimizer.minimize(loss)

with tf.name_scope('init'):
    init = tf.global_variables_initializer()

sess = tf.Session()
merged = tf.summary.merge_all()
writer = tf.summary.FileWriter("logs3/", sess.graph)
sess.run(init)

for step in range(101):
    sess.run(train)
    rs = sess.run(merged)
    writer.add_summary(rs, step)

```

***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
