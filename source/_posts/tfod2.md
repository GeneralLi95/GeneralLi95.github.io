---
title: 物体检测TensorFlow Object Detection API （二）使用 Jupyter Notebooks 学习官方 demo
date: 2018-08-10 14:55:41
catalog: true
tags: 
    - tensorflow
    - 机器学习
---
# 物体检测TensorFlow Object Detection API （二）使用 Jupyter Notebooks 学习官方 demo

jupyter notebooks 之前也被称为 iPython 笔记本，提供了在同一环境中执行数据可视化的功能，是数据科学家最常用的工具之一。

关于 Jupyter Notebooks 的使用可以看公众号机器之心的一篇科普文章 [入门｜始于Jupyter Notebooks：一份全面的初学者实用指南](http://baijiahao.baidu.com/s?id=1601883438842526311&wfr=spider&for=pc) 

综合来看，Jupyter Notebooks 非常适合教学和演示。所以 Google 开发人员也写了一个教学的 Jupyter 脚本，来帮我们演示。

Jupyter Notebooks 笔记文件的后缀名都是 .ipynb

终端输入：

```
jupyter notebook
```

即可启动 jupyter notebook

![](http://p6atp7tts.bkt.clouddn.com/15338823163485.jpg)

在系统8888端口，jupyter notebooks 已经跑起来了。
一般来说会自动打开浏览器，如果没有，自己打开浏览器 输入 localhost: 8888 即可。

![](http://p6atp7tts.bkt.clouddn.com/15338824038441.jpg)

启动界面显示的是当前所在目录，找到位于  /models/research/object_detection/object_detection_tutorial.ipynb

![](http://p6atp7tts.bkt.clouddn.com/15338825675427.jpg)
点击 run all 整个代码就跑起来了。

如果前一篇文章最后的测试代码，能够输出 OK 的话，证明安装无误，这个文档跑起来应该是没问题的，如果有问题自行解决。

大概运行3-5分钟，即可看到结果了。
    > 运行时间引人而异，在 [目标检测Tensorflow object detection API](https://zhuanlan.zhihu.com/p/35795901) 这篇文章中，这个同学讲他用了30-40分钟。因为中间有几行代码是下载训练好的模型，所以和网速也会有关系。

![](http://p6atp7tts.bkt.clouddn.com/15338828519716.jpg)

![](http://p6atp7tts.bkt.clouddn.com/15338828651205.jpg)


关于这个官方 demo，不在具体解释。如果我们把整个任务分成下面部分

1. 准备数据集
2. 训练模型
3. 测试模型


3个大部分的话，这个 demo 应该算第三部分，用训练好的模型在其他图片是进行 object_detection.

在具体工作中，大部分工作量其实是集中在前两个步骤。

***
本文首发于个人网页 [Yao Blog](http://liyaolife.com)，知乎专栏 [谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)。


