---
title: 在Python下搭建QT+SIP+PyQt5环境
date: 2018-08-13 16:04:35
tags:
  - Python
categories: Python
catalog: true
---

PyQt是Python界面开发的常用库，因为需要写一个图像标注的GUI界面，所以用到了这个库。但是在环境搭建的实际过程中，查阅了大量的资料，尝试了很多种方法，大都以失败告终。在此将最后的解决方案记录下来。

## 1. 安装QT
Qt 是一个跨平台的 C++ 应用程序开发框架，是自由且开放源代码的软件

> Qt [1]  是一个1991年由Qt Company开发的跨平台C++图形用户界面应用程序开发框架。它既可以开发GUI程序，也可用于开发非GUI程序，比如控制台工具和服务器。Qt是面向对象的框架，使用特殊的代码生成扩展（称为元对象编译器(Meta Object Compiler, moc)）以及一些宏，Qt很容易扩展，并且允许真正地组件编程。2008年，Qt Company科技被诺基亚公司收购，Qt也因此成为诺基亚旗下的编程语言工具。2012年，Qt被Digia收购。2014年4月，跨平台集成开发环境Qt Creator 3.1.0正式发布，实现了对于iOS的完全支持，新增WinRT、Beautifier等插件，废弃了无Python接口的GDB调试支持，集成了基于Clang的C/C++代码模块，并对Android支持做出了调整，至此实现了全面支持iOS、Android、WP,它提供给应用程序开发者建立艺术级的图形用户界面所需的所有功能。基本上，Qt 同 X Window 上的 Motif，Openwin，GTK 等图形界 面库和 Windows 平台上的 MFC，OWL，VCL，ATL 是同类型的东西。
> ——百度百科


安装 qt 的方式基本上分两种

### 1.1 通过 brew 安装 qt
使用 macOS 的开发人员一定知道brew这个非常好用的包管理工具，基本上只要能在mac上安装的东西，都可以通过 homebrew 来安装和管理，并且可以及时对其进行更新和下载。

通过brew安装的办法：

```shell
brew install qt
```

**优点：** 一行代码搞定，安装包精简，同时速度快。

**缺点：** brew下载下来的包大小只有100MB左右，解压后也只有300多兆。而官网的.dmg安装包有一个G，全套组件安装下来占用空间将近13G。所以在后续过程中可能还需要使用brew安装其他东西。

### 1.2 通过官网安装
官网的开源版本下载页面迟迟打不开，令人十分急躁，并且似乎还要注册。所以此处推荐大家使用镜像资源下载。（没有链接，自己找最新版的吧。）

官网安装基本上就是一路下一步即可了。

**缺点：** 安装包太大，并且后续安装不能使用Homebrew了。因为通过brew安装sip或者PyQt的时候会检查系统有没有qt，而如果是通过官网安装的qt，不在brew目录下面的话，它就会重新执行**brew install qt**这样相当于官网安装的就没有用上。
## 2. 安装SIP
> sip: create python bindings for c and c++ libraries

sip是RiverBank（也就是PyQt的开发商）开发的用于PyQt的Python/C++混合编程解决方案。由于Qt框架的复杂性，PyQt并没有使用Cython、SWIG的混合编程方案，而是自己单独做了一套框架。sip包括一个sip工具、SDK和Python Module。

与SWIG类似，使用sip也需要先编写一个「配置文件」，然后使用sip工具『编译』为C++源文件，最后，和Qt库一起编译形成适用于Python的PyQt。

与SWIG不同的是，sip 同时以Python Module的形式存在，也就是说，作为Python Module的PyQt，依赖于作为Python Module的sip。而对于SWIG，一旦自动生成的C++生成完毕，整个流程就不再依赖SWIG了。

很多教程推荐使用 brew 安装 sip， 这样省编译 blabla 因为，但实际上我们上面已经交代了， sip 同时以 Python Module 的形式存在，所以，此时可以使用 pip 工具了

```shell
pip3 install sip
```

搞定。


## 3. 安装PyQt5

PyQt是python的一个插件库,通过这个库我们可以连接qt和python.便捷的使用GUI编程.

```shell
pip3 install pyqt5
```

安装好了。

## 4. 配置 PyCharm 开发环境

1. 先确认我们的 pyqt 模块是不是安装好了， 以及python版本。


2. 配置GUI设计工具

![15332767318097](https://upload-images.jianshu.io/upload_images/11400909-a15901baa3e37bcf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这里要求我们在安装qt的时候，能够知道其路径在何处。
Working Directory是系统自动生成的，不需要我们设置。

3. 设置 ui 文件编译工具

![15332769446963](https://upload-images.jianshu.io/upload_images/11400909-6c4d6dab2672a1b9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Parameters 栏的固定代码：

```
-m PyQt5.uic.pyuic  $FileName$ -o $FileNameWithoutExtension$.py
```

Ps: Working directory 栏也会自动生成路径，但是有时候生成的不是那个位置，而是qt文件位置或其他，我们需要把它改成我们那样，这样相当于一个定位，让编译程序在本 project 内部找编译的源文件。

4. 安装好之后的模样：

![image](https://upload-images.jianshu.io/upload_images/11400909-9e78d991eafc0248.png)



## 5. 开始使用 PyQt5 编写第一个 GUI 程序

 这部分不再赘述，看下面的参考资料就可以啦。
 [momoxiaomming的博文：如何在Python下搭建QT+SIP+PyQt5环境](https://juejin.im/post/5a671677518825734501ad2e)

 总之基本上步骤是：

 1. 使用 QT_Designer 图形化界面，拖动的方式设计。 生成.ui文件
 2. 使用 PyGUI 将 .ui 文件编译成 .py 文件
 3. 编写脚本 import 前面生成的 .py 文件，再有一通操作即可。

***

参考资料：
[momoxiaomming的博文：如何在Python下搭建QT+SIP+PyQt5环境](https://juejin.im/post/5a671677518825734501ad2e)

[Qt安装后配置环境变量（Mac）](http://www.cnblogs.com/goodboy-heyang/p/4793459.html)

[善用Homebrew](http://zhailiange.com/2016/06/04/Homebrew/)

***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
