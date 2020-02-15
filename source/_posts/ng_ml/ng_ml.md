---
title: Andrew Ng Machine Learning - 1 Intorduction
date: 2019-12-28 23:07:34
tags:
  - Andrew Ng Machine Learning
  - MOOC
categories:
  - 机器学习
mathjax:
---

开始在 Coursera上学习 斯坦福吴恩达的机器学习教程，是侧重神经网络的新版，不是侧重 SVM 的旧版。
## 什么是机器学习
首先 Ng 欢迎来到课程，介绍什么是 Machine Learning 这个视频很有名了，在两个版本中都介绍了这一部分。举了一些例子，包括他的学生做的直升飞机，无人车什么的。不再详述。
## 监督学习与无监督学习
分类 机器学习 按照大类可分为两类 Supervised Learning 和 Unsupervised Learning，即监督学习和无监督学习。那么当下比较火的是 自监督学习。不知道应该属于两者中的哪一类。

在监督学习部分，举了几个例子
* 房价预测的例子，线性回归模型， Regression problem
* breast cancer（malignant, benign) 乳腺癌，恶性与良性的判断，Classification problem

 quiz:

 You’re running a company, and you want to develop learning algorithms to address each of two problems. Problem 1:You have a large inventory of identical items. You want to predict how many of these items will sell over the next 3 months.

 Problem 2: You’d like software to examine individual customer accounts, and for each account decide if it has been hacked/compromised. Should you treat these as classification or as regression problems?

* Treat both as classification problems.
* Treat problem 1 as a classification problem, problem 2 as a regression problem.
* **Treat problem 1 as a regression problem, problem 2 as a classification problem.**
* Treat both as regression problems.

这个问题就是给两个问题判断属于哪一类，第一类是预测投资对后三个月的销售影像，典型的回归问题，第二个是用程序检测用户账户是否被入侵，典型的分类问题。

辅助材料给出了 监督学习、回归问题、分类问题的定义

>In supervised learning, we are given a data set and already know what our correct output should look like, having the idea that there is a relationship between the input and the output.

>Supervised learning problems are categorized into "regression" and "classification" problems. In a regression problem, we are trying to predict results within a continuous output, meaning that we are trying to map input variables to some continuous function. In a classification problem, we are instead trying to predict results in a discrete output. In other words, we are trying to map input variables into discrete categories.
