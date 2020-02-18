---
title: Andrew Ng Machine Learning - 1 Intorduction
date: 2019-12-28 23:07:34
tags:
  - Andrew Ng Machine Learning
  - MOOC
categories:
  - Machine Learning
  - Stanford Andrew Ng Machine Learning Course
mathjax:
---

开始在 Coursera上学习 斯坦福吴恩达的机器学习教程，是侧重神经网络的新版，不是侧重 SVM 的旧版。
## 什么是机器学习
首先 Ng 欢迎来到课程，介绍什么是 Machine Learning 这个视频很有名了，在两个版本中都介绍了这一部分。举了一些例子，包括他的学生做的直升飞机，无人车什么的。不再详述。
## 1.1 Supervised Learning
分类 机器学习 按照大类可分为两类 Supervised Learning 和 Unsupervised Learning，即监督学习和无监督学习。那么当下比较火的是 自监督学习。不知道应该属于两者中的哪一类。

在监督学习部分，举了几个例子
* 房价预测的例子，线性回归模型， Regression problem
* breast cancer（malignant, benign) 乳腺癌，恶性与良性的判断，Classification problem

 **quiz:**

 You’re running a company, and you want to develop learning algorithms to address each of two problems. Problem 1:You have a large inventory of identical items. You want to predict how many of these items will sell over the next 3 months.

 Problem 2: You’d like software to examine individual customer accounts, and for each account decide if it has been hacked/compromised. Should you treat these as classification or as regression problems?

* Treat both as classification problems.
* Treat problem 1 as a classification problem, problem 2 as a regression problem.
* **Treat problem 1 as a regression problem, problem 2 as a classification problem.**
* Treat both as regression problems.

这个问题就是给两个问题判断属于哪一类，第一类是预测投资对后三个月的销售影像，典型的回归问题，第二个是用程序检测用户账户是否被入侵，典型的分类问题。
## 1.2 Unsupervised Learning
无监督学习，举了几个例子，谷歌新闻 group them into cohesive news stories，将新闻按主题大致分类。基因检测。列举了一些无监督学习的应用，如组织计算机集群，社交网络分析，用户属性分类，太空数据分析等等。

然后是经典的鸡尾酒会例子，两个人，两个麦克风。距离不同。两个人同时数1到10.通过无监督学习将这两个声音区分开。
![image.png](https://i.loli.net/2020/02/18/iId41FlgpqMWExk.png)

同时这里介绍了编程软件 Octave。可以简单认为是一个免费的 Matlab。
> cohesive 内聚，凝聚的
> genomics 基因
> cocktail party 鸡尾酒会

**Quiz**

Of the following examples, which would you address using an unsupervised learning algorithm? (Check all that apply.)

* Given email labeled as spam/not spam, learn a spam filter.
* **Given a set of news articles found on the web, group them into sets of articles about the same stories.**
* **Given a database of customer data, automatically discover market segments and group customers into different market segments.**
* Given a dataset of patients diagnosed as either having diabetes or not, learn to classify new patients as having diabetes or not.

这个问题是判断监督学习与无监督学习，都是课程视频里面讲过的例子，答案显而易见。

## 1.3 Review
辅助材料给出了 监督学习、回归问题、分类问题的定义

>In supervised learning, we are given a data set and already know what our correct output should look like, having the idea that there is a relationship between the input and the output.

>Supervised learning problems are categorized into "regression" and "classification" problems. In a regression problem, we are trying to predict results within a continuous output, meaning that we are trying to map input variables to some continuous function. In a classification problem, we are instead trying to predict results in a discrete output. In other words, we are trying to map input variables into discrete categories.

## 1.4 章节测试
1. A computer program is said to learn from experience E with respect to some task T and some performance measure P if its performance on T, as measured by P, improves with experience E. Suppose we feed a learning algorithm a lot of historical weather data, and have it learn to predict weather. In this setting, what is T?

    * None of these.
    * The probability of it correctly predicting a future date's weather.
    * **The weather prediction task.**
    * The process of the algorithm examining a large amount of historical weather data.


2. Suppose you are working on weather prediction, and use a learning algorithm to predict tomorrow's temperature (in degrees Centigrade/Fahrenheit). Would you treat this as a classification or a regression problem?

    * Classification
    * **Regression**

    预测明日温度，应属于 Regression ，而第6小问，预测明天5点是否会下雨，是 Classification

3. Suppose you are working on stock market prediction, and you would like to predict the price of a particular stock tomorrow (measured in dollars). You want to use a learning algorithm for this. Would you treat this as a classification or a regression problem?

    * **Regression**
    * Classification

4. Some of the problems below are best addressed using a supervised learning algorithm, and the others with an unsupervised learning algorithm. Which of the following would you apply supervised learning to? (Select all that apply.) In each case, assume some appropriate dataset is available for your algorithm to learn from.


    * Given a large dataset of medical records from patients suffering from heart disease, try to learn whether there might be different clusters of such patients for which we might tailor separate treatments.
    * Given data on how 1000 medical patients respond to an experimental drug (such as effectiveness of the treatment, side effects, etc.), discover whether there are different categories or "types" of patients in terms of how they respond to the drug, and if so what these categories are.
    * Have a computer examine an audio clip of a piece of music, and classify whether or not there are vocals (i.e., a human voice singing) in that audio clip, or if it is a clip of only musical instruments (and no vocals).
    * **Given genetic (DNA) data from a person, predict the odds of him/her developing diabetes over the next 10 years.**
    * **In farming, given data on crop yields over the last 50 years, learn to predict next year's crop yields.**
    * Examine a web page, and classify whether the content on the web page should be considered "child friendly" (e.g., non-pornographic, etc.) or "adult."
    * Take a collection of 1000 essays written on the US Economy, and find a way to automatically group these essays into a small number of groups of essays that are somehow "similar" or "related".
    * **Examine the statistics of two football teams, and predict which team will win tomorrow's match (given historical data of teams' wins/losses to learn from).**
    * Examine a large collection of emails that are known to be spam email, to discover if there are sub-types of spam mail.
    <font color = #FF7F00>这道题每次新做都会有一些新的选项，我做了三遍应该穷尽了大部分选项，很有迷惑性的一道题，一般看到给出了大量数据，尤其是以时间为先做的数据的时候往往容易直接选成 Regression，但是实际上，第一个选项要分 cluster，第二个选项， different categories，以及最后一个垃圾短信的，课程中垃圾短信是一个 Regression，而这个选项是寻找垃圾短信的 sub-types。实很有迷惑性。</font>

5. Which of these is a reasonable definition of machine learning?

    * Machine learning is the science of programming computers.
    * Machine learning is the field of allowing robots to act intelligently.
    * Machine learning learns from labeled data.
    * **Machine learning is the field of study that gives computers the ability to learn without being explicitly programmed.**

6. Suppose you are working on weather prediction, and you would like to predict whether or not it will be raining at 5pm tomorrow. You want to use a learning algorithm for this. Would you treat this as a classification or a regression problem?
    * **Classification**
    * Regression
