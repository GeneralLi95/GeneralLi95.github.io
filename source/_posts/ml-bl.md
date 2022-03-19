---
title: 北京理工大学慕课「Python与机器学习」笔记
date: 2017-11-24 16:34:05
tags:
  - Machine Learning
categories:
  - Machine Learning 
mathjax:
---

## 1 绪论

机器学习分类
> * 监督学习
> * 无监督学习
>   训练集无人为标注结果
>
> * 强化学习
> * 半监督学习
> * 深度学习
> 当下最火的机器学习应用
>

Python Scikit-Learn库

scikit-learn常用算法的调用方法

  |应用|算法|
---|---|---|
分类(classfication)|异常检测、图像识别等|KNN，SVM，etc
聚类(clustering)|图像分割、群体划分等|K-Means，谱聚类，etc
回归(regression)|价格预测，趋势预测，等|线性回归，SVR，etc
降维|可视化|PCA，NMF，etc

## 2 sklearn 安装

scikit-learn简称，是在Numpy、Scipy和matplotlib基础上开发而成，因此使用sklearn库之前需要先安装前几个库

```
Numpy（Numerical Python）是一个开源的Python科学计算库

Scipy库是sklearn库的基础，它是基于Numpy的一个集成了多种数学算法和函数的Python模块

matplotlib是基于Numpy的一套Python工具包，提供了大量的数据绘图工具

安装顺序
Numpy
Scipy
matplotlib
sklearn
```

由于mac系统自带的Python是2.7，自带的包也是只支持2.x版本的，要使用Python3的话需要重新安装这几个包。

**如果要为某一脚本选择Python2或者Python3解释器，只需要在脚本前面加入一行**

`# ！Python2`或
`# ! Python3`即可
在命令行环境下输入下列代码(不需要先进入python 环境)

```
pip3 install numpy #安装支持Python3的的Numpy包
pip3 install scipy #安装支持Python3的scipy包
pip3 install matplotlib  #安装支持Python3 的matplotlib包
pip3 install sklearn  #安装sklearn包

```

数据下载 ：uzi数据集

## 3 sklearn库中的标准数据集及基本功能

数据集总览

-|数据集名称|调用方式|适用算法|数据规模
---|---|---|---|---
小数据集|波士顿房价数据集|load_boston()|回归|506*13
-|鸢尾花数据集|load_iris()|分类|150*4
-|糖尿病数据集|load_diabetes()|回归|442*10
-|手写数字数据集|load_digits()|分类|5620*64
大数据集|Olivetti脸部图像数据集|fetch_olivetti_faces()|降维|400 * 64 * 64
-|新闻分类数据集|fetch——20newsgroups（）|分类|-
-|带标签的人脸数据集|fetch_lfw_people()|分类；降维|-
-|路透社新闻语料数据集|fetch_revl()|分类|804414*47236

*注：小数据集可以直接使用，大数据集要在调用程序时自动下载（一次即可）*

波士顿房价数据集：
包含506组数据，每条数据包含房屋以及房屋周围的详细信息。其中包括城镇犯罪率、一氧化碳浓度、住宅平均房间数、到中心去域的加权距离以及自主房平均房价等。因此，波士顿房价数据集能够应用到回归问题上。

例如
使用
`sklearn.datasets.load_bosten`即可加载相关数据集
重要参数
`return_X_y`:表示是否返回target(即价格），默认为FALSE，只返回data(即属性）

波士顿房价数据集-加载示例

```
>>> from sklearn.datasets import load_boston
>>> boston = load_boston()
>>> print(boston.data.shape)
(506,13)
```

示例二

```
>>> from sklearn.datasets import load_boston
>>> data,target = load_boston(return_X_y=True)
>>> print(data.shape)
(506,13)
>>> print(target.shape)
506
```


手写数字数据集
包括1797个0-9的手写数字数据，每个数字由8*8大小的矩阵构成，矩阵中值得范围是0-16，代表颜色的深度。

示例

```
>>> from sklearn.datasets import load_digits
>>> digits = load_digits()
>>> print(digits.data.shape)
(1797,64)
>>> print(digits.target.shape)
(1797,)
>>> print(digits.images.shape)
(1797.8.8)
>>> import matplotlib.pyplot as plt
>>> plt.matshow(digits.images[0])
>>> plt.show()
```
---
### sklearn库的基本功能
sklearn库的功能分为6大部分，分别用于完成分类任务、回归任务、聚类任务、降维任务、模型选择以及数据的预处理。

分类任务

分类模型|加载模块
---|---
最近邻算法|neighbors.NearestNeighbors
支持向量机|svm.SVC
朴素贝叶斯|naive_bays.GaussianNB
决策树|tree.DecisionTreeClassifier
集成方法|ensemble.BaggingClassifier
神经网络|neural_network.MLPClassifier

回归任务

回归模型|加载模块
---|---
岭回归|linear_model.Ridge
Lasso回归|linear_model.Lasso
弹性网络|linear_model.ElasticNet
最小叫回归|linear_model.Lars
贝叶斯回归|linear_model.BayesianRidge
逻辑回归|linear_model.LogisticRegression
多项式回归|preprocessing.PolynomialFeatures

聚类任务

聚类方法|加载模块
---|---
K-means|cluster.KMeans
AP聚类|cluster.AffinityPropagation
均值漂移|cluster.MeanShift
层次聚类|cluster.AgglomerativeClustering
DBSCAN|cluster.DBSCAN
BIRCH|cluster.Birch
谱聚类|cluster.SpectralClustering

降维任务

降维方法|加载模块
---|---
主成分分析|decomposition.PCA
截断SVD和LSA|decomposition.TruncatedSVD
字典学习|decomposition.SparseCoder
因子分析|decomposition.FactorAnalysis
独立成分分析|decomposition.FastICA
非负矩阵分解|decompositon.NMF
LDA|decompositon.LatentDirichletAllocation

## 4 无监督学习

利用无标签的数据学习数据的分布或数据与数据之间的关系。

*有监督学习和无监督学习的最大区别在于数据是否有标签*
无监督学习最常用的场景是聚类和降维。

**聚类，就是根据数据的“相似性”将数据分为多类的过程。**
so，如何定义相似性？
根据两个样本之间的距离。
> 欧式距离 几何距离
>
> 曼哈顿距离  也称街区距离 个人理解就是直角三角形两直角边的和
>
> 马氏距离  表示数据的协方差距离，是一种尺度无关的度量方式。也就是说马氏距离会先将样本点的各个属性标准化，再计算样本间的距离。 （*感觉马氏距离比较难理解*）
>
> 余弦相似度 用向量空间中两个向量夹角的余弦值作为衡量两个样本差异的大小。余弦值越接近1，说明两个向量夹角越接近0度，说明两个向量越相似。

以同样的数据集应用于不同的算法可能会得到不同的结果，这是由算法特性决定的。

sklearn.cluster
数据标准输入格式：[样本个数，特征个数]定义的矩阵形式。
相似度矩阵输入格式

sklearn.cluster几个有代表性的聚类方法的参数与特性

算法名称|参数|可扩展性|相似性度量
---|---|---|---
K-means|聚类个数|大规模数据|点间距离
DBSCAN|邻域大小|大规模数据|点间距离
Gaussian Mixtures|聚类个数及其他超参|复杂度高，不适合处理大规模数据|马氏距离
Birch|分支因子，阈值等其他超参|大规模数据|两点间的欧式距离


**降维，就是在保证数据所具有的代表性特性或者分布的情况下，将高维数据转化为低位数据的过程。**

通常用于：
> 数据可视化
> 精简数据，中间过程，提高机器学习算法效率

降维过程也可以理解为对数据集的组成成分进行分解（decomposition）的过程，因此sklearn为降维模块命名为decomposition，在对降维算法调用需要使用sklearn.decomposition模块。


sklearn.decomposition里的几个常用降维算法

算法名称|参数|可拓展性|适用任务
---|---|---|---
PCA|所降维度及其他超参|大规模数据|信号处理等
FastICA|所降维度及其他超参|超大规模数据|图形图像特征提取
NMF|所降维度及其他超参|大规模数据|图形图像特征提取
LDA|所降维度及其他超参|大规模数据|文本数据，主题挖掘


## 5 K-means 聚类算法

以k为参数，把n个对象分成k个簇，使簇内具有较高的相似度，而簇间的相似度较低
> 1.随机选择k个点作为初始的聚类中心
> 2.对于剩下的点，根据其与聚类中心的距离，将其归入最近的簇
> 3.对每个簇，计算所有点的均值作为新的聚类中心
> 4.重复2、3直到聚类中心不再发生改变

## 6 DBSCAN 密度聚类算法

基于密度进行聚类，一种基于密度进行聚类的算法。
> 聚类的时候不需要预先指定簇的个数
> 最终的簇的个数不确定

DBSCAN算法将数据点分为三类：
核心点：在半径Eps内含有超过MinPts数目的点。
边界点：在半径Eps内点的数量小于MinPts，但是落在核心点的邻域内。
噪音点：既不是核心点，也不是边界点。

算法流程：
> 1.将所有点标记为核心点、边界点或噪声点；
> 2.删除噪声点；
> 3.为距离在Eps之内的所有核心点之间赋予一条边；
> 4.每组连通的核心点形成一个簇；
> 5.将每个边界点指派到一个与之关联的核心点的簇中（即看它在哪一个核心点的半径范围之内，那么它就属于该核心点所在的簇）。


## 7 主成成分分析（PCA）降维算法
Principal Component Analysis，PCA是一种常用的降维方法，通常用于高维数据集的探索与可视化，还可以用作数据压缩和预处理等。

回顾统计学名词：
方差：
协方差：用于度量两个变量之间的线性相关性程度
特征向量：描述数据集结构的非零向量

PCA的原理：
矩阵的主成分就是其协方差矩阵对应的特征向量，按照对应的特征值大小进行排序，最大的特征值就是第一主成分，其次是第二主成分，以此类推。（推到过程参见《机器学习》南京大学周志华。）

## 8 非负矩阵分解（NMF） 降维方法
非负矩阵分解（Non-negative Matrix Factorization)是在矩阵中的元素均为非负数约束条件之下的非负矩阵分解方法。

基本思想：给定一个非负矩阵V，NMF能够找到一个非负矩阵W和一个非负矩阵H，是得矩阵W和H的乘积近似等于矩阵V中的值。

> W 矩阵：基础图像矩阵，相当于从原矩阵V中抽取出来的特征。
> V 矩阵：系数矩阵

NMF能够广泛应用于图像分析、文本挖掘、和图像处理领域。
