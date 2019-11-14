---
title: tensorflow安装CPU指令集（AVX2）警告解决方案
date: 2018-05-25 09:19:08
tags:
    - 机器学习
    - tensorflow
catalog: true
---
## 问题原因
在macOS通过pip3 install 安装tensorflow(CPU版)后，运行示例代码

```
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello).decode())
```

运行之后可以正常输出

“Hello, TensorFlow!"

但是有一个警告警告提示：

```
 I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
```

在Stack Overflow上面关于这个问题有[一个详细的解答](https://stackoverflow.com/questions/47068709/your-cpu-supports-instructions-that-this-tensorflow-binary-was-not-compiled-to-u)

大致翻译一下高赞答案：
>Modern CPUs provide a lot of low-level instructions, besides the usual arithmetic and logic, known as extensions, e.g. SSE2, SSE4, AVX, etc. From the Wikipedia:

> Advanced Vector Extensions (AVX) are extensions to the x86 instruction set architecture for microprocessors from Intel and AMD proposed by Intel in March 2008 and first supported by Intel with the Sandy Bridge processor shipping in Q1 2011 and later on by AMD with the Bulldozer processor shipping in Q3 2011. AVX provides new features, new instructions and a new coding scheme.
In particular, AVX introduces fused multiply-accumulate (FMA) operations, which speed up linear algebra computation, namely dot-product, matrix multiply, convolution, etc. Almost every machine-learning training involves a great deal of these operations, hence will be faster on a CPU that supports AVX and FMA (up to 300%). The warning states that your CPU does support AVX (hooray!).
>I'd like to stress here: it's all about CPU only.


现代CPU提供了一系列低级别的指令集，除了通常的算术和逻辑之外，被称为扩展，例如， SSE2，SSE4，AVX等。
维基百科有描述：
> 高级矢量扩展（AVX）是英特尔在2008年3月提出的英特尔和AMD微处理器的x86指令集体系结构的扩展，英特尔首先通过Sandy Bridge处理器在2011年第一季度推出，随后由AMD推出Bulldozer处理器 在2011年第三季度.AVX提供了新功能，新指令和新编码方案。


特别是，AVX引入了融合乘法累加（FMA）操作，加速了线性代数计算，即点积，矩阵乘法，卷积等。几乎所有机器学习训练都涉及大量这些操作，因此将会 支持AVX和FMA的CPU（最高达300％）更快。 该警告指出您的CPU确实支持AVX（hooray！）。

**那为什么没有使用呢？**

由于tensorflow默认是在没有CPU扩展的情况下构建的，例如SSE4.1，SSE4.2，AVX，AVX2，FMA等。默认版本（来自pip install tensorflow的版本）旨在与尽可能多的CPU兼容。 另一个观点是，即使使用这些扩展名，CPU的速度也要比GPU低很多，所以它预计中大型机器学习任务应该在GPU上执行。

**那我们应该怎么办**

如果你有GPU的话，直接忽略这一项即可，因为大部分的高消耗操作都会被分配到GPU上执行（除非你设置了不这么做）。在这种情况下，你可以通过下面这个方式直接忽略这个警告：

```
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
```


这种方法只是去掉警告，实际上是没有解决问题的。

如果你没有GPU，你想最大化使用CPU，你应该启动你的CPU的AVX，AVX2以及FMA拓展。在[这个问题](https://stackoverflow.com/questions/41293077/how-to-compile-tensorflow-with-sse4-2-and-avx-instructions)以及这个[GitHub issue](https://github.com/tensorflow/tensorflow/issues/8037)里面都有详细的讨论。Tensorflow使用称为bazel的ad-hoc构建系统，构建它并不是那么简单，但肯定是可行的。 在此之后，不仅警告消失，张量流性能也应该改善。


## 解决方法
**什么是SSE4.2和AVX？**

**SIMD** (Single Instruction Multiple Data)单指令流多数据流，是一种采用一个控制器来控制多个处理器，同时对一组数据（又称“数据向量”）中的每一个分别执行相同的操作从而实现空间上的并行性技术。

在微处理器中，SIMD则是一个控制器控制多个并行的处理微元，例如Intel的MMX或SSE，以及AMD的3D NOW指令集。

所以说SSE4.2和AVX都是一种SIMD指令集。

对于TF tasks。SSE4.2和AVX使向量和矩阵计算更加高效。具体可以看这个[课件](https://www.polyhedron.com/web_images/intel/productbriefs/3a_SIMD.pdf)。

所以该怎么做，大概只能重装一遍tensorflow了。注意这次重装时候要从源码安装，从在源码安装的时候进行相关的设置。

[从源码安装的官方教程](https://tensorflow.google.cn/install/install_sources)总体来说还是非常麻烦的，需要很多依赖，安装一些其他的东西。

基本过程可以概括为三步：
1. 下载TensorFlow源码
2. 准备安装环境 （此处需要安装很多东西）
3. 构建pip软件包（一个.whl后缀文件）
4. 使用pip命令进行本地安装

其中2，3步骤都非常负责，及其容易出错。

最终采用的是另外一种方法。
到这个[GitHub repo](https://github.com/lakshayg/tensorflow-build)下载自己对应版本的pip软件包。

<font color =red>一定要对应版本!</font>

版本查看方式

```
$ python3

Python 3.6.5 (default, Apr 25 2018, 14:23:58)
[GCC 4.2.1 Compatible Apple LLVM 9.1.0 (clang-902.0.39.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```


从中可以得出我们的版本  Python 3.6.5 clang-902.0.29.1，又这两个版本号加上自己的系统名在上面的GitHub repo里面选择对应的软件包。下载下来相当于直接完成了官网教材的前3步。

所以只需要执行第4步。

```
pip install --ignore-installed --upgrade /path/to/binary.whl
```

注意 Python3的pip命令是pip3  这条语句可以忽略我们已经安装好的tf包，不需要卸载，会直接升级过去。很方便。


***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
