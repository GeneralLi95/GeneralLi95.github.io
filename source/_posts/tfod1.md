---
title: 物体检测TensorFlow Object Detection API （一）安装
date: 2018-08-10 14:55:35
tags:
    - TensorFlow
    - 机器学习
    - 计算机视觉
    - 目标检测
categories: 机器学习
catalog: true
---

在计算机视觉任务中，区分一下图像分类和物体检测，一部分任务的数据标注形式是

```
图片-标签1，标签2，标签3
```

这种类型的数据，我们最终的目的，也是测试的图片，打标签，姑且将其认为属于图片分类任务。

而另外一种数据

```
图片中的某一块-标签1
图片中的另一块-标签2
```

这种任务，我们的目的是在某张图片中检测（查找）某物体。

TensorFlow Object Detection API 是 TensorFlow models 里的一个 research project 其中预设了很多网络模型可供我们直接调用和调参，也可以根据其自定义模型。大大简化了我们进行实验的流程。然而，即便如此，TensorFlow 依然不是一个新 friendly 的一个项目。在开发过程中可能遇到各种各样的问题。在此做此记录。
——2018.8.10

## 安装
此处参考了 [ Installation](
https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md) TensorFlow models 官方 GitHub doc.

### 依赖

* Protobuf >= 3.0.0
* Python-tk
* Pillow
* lxml
* tf Slim
* Jupyter notebook
* matplotlib
* TensorFlow
* Cython
* contextlib2
* cocoapi

TensorFlow 如何安装不再说明，Protobuf 可通过 brew 安装。
coco api 选装
其余通过 pip 安装

其中 probuf 被用来设置模型和训练参数，在正式使用前，需要将 protobuf 库进行编译

```shell
# From tensorflow/models/research/
protoc object_detection/protos/*.proto --python_out=.
```

注意看好路径。

jupyter notebook 是交互式的笔记本应用，可以边写代码边记笔记。非常实用的工具，使用它可以看作者预设的一个最简单的模型。

### 将 slim directories 加入 PYTHONPATH

```
# From tensorflow/models/research/
export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
```
任何位置使用此 api 都要首先运行此命令。

如果没有运行此命令可能会出现

```shell
ModuleNotFoundError: No module named 'object_detection'
```
这样的错误

###  测试安装效果

```shell
python object_detection/builders/model_builder_test.py
```
因为在本人机器上 python 命令默认调用 python2， python3 命令才会调用 python3 所以在测试命令改为：

```shell
python3 object_detection/builders/model_builder_test.py

```

如果出现以下场景，表明运行成功。

![image](http://upload-images.jianshu.io/upload_images/11400909-ef5f5ca8c03c5218.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

代表安装成功了。

> 一件趣事：
> 在测试这个8月6号，7号测试时候，总是出现错误，后来发现是一处 xrange（python2）用法没有改成 range。 然后我把它改了就能运行了，发了 pull request 被 Google 的哥哥回复了，说他们正在更新一个大版本，里面已经改了这个错误，然后 8月8号就确实更新了一个新版本，改了很多地方。


***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
