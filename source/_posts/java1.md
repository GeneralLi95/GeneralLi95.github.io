---
title: Mac Java环境配置
date: 2018-05-19 13:42:32
tags:
  - Java
  - 编程
categories: Java
catalog: true
---
# MAC Java环境配置

## 1. 安装JDK
JDK全称Java Development Kit，Java开发工具包，是Java开发的核心。
之前学Oracle装过JDK。所以这次只在终端查看一下是否装过。
终端输入

```
Java -version
```

显示如下画面，既是安装成功，如果没有安装，到Oracle官网下载对应的包，一路下一步安装即可，比较容易。
![image](http://upload-images.jianshu.io/upload_images/11400909-e41bcc47495420eb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 2. 配置环境变量
配置环境变量前先得到JAVA_HOME的位置。
依旧：终端输入：

```
/usr/libexec/java_home
```
可返回JAVA_HOME的位置路径，把这个路径记下来，马上要用

```
/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home
```

环境变量一共有三个东西需要配置：
JAVA_HOME
CLASS_PATH
PATH
在windows 系统里是在我的电脑属性高级设置里分别设置，macOS里依然是通过命令行设置。
终端输入：

```
sudo vim /etc/profile
```
然后后需要输入密码。接下来按i，显示insert，进入输入模式。然后输入下面配置：

```
JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home"
export JAVA_HOME
CLASS_PATH=i"$JAVA_HOME/lib"
PATH=".$PATH:$JAVA_HOME/bin"
```
然后按esc，进入保存。
输入```:wq!``` 保存

<font color=red>
这里注意一下，JAVA_HOME的路径不要照着网上教程全抄了，把自己刚刚存下来的路径copy过来，毕竟安装的JDK版本不一致的时候，路径当中有的文件的名字是不一致的。
</font>

接下来 输入

```
source /etc/profile
```
运行profile配置文件

检查环境
输入

```
echo $JAVA_HOME
```
得到配置的路径，说明配置完毕。

> 5.14 日更新：
> 发现该环境变量配置之后，终端许多命令无法执行，比如python3,pip以及hexo命令，这是无法忍受的。所以又强行把它改了回去，得以恢复。
> 后发现IntelliJ IDEA会自带Java环境，在不设置这个环境变量的情况下，Java的hello world还是跑起来了。
> 所以真的是**生命在与折腾！**


## 选择IDE

有些人可能建议初学者用记事本写代码，我感觉初学者最重要的应该是马上跑起来一个程序得到及时的反馈，所以我还是习惯用IDE。

Java的IDE也挺多的，比较有名的是Eclipse和IntelliJ IDEA两款，作为一个初学者，当然一个也没用过，前者开源后者付费。考虑到后者是JetBrains的产品，他们公司的Pycharm和Webstorm我是用过并且非常喜欢的，所以上手IntelliJ IDEA可能会比较容易的，另外JetBrains也向学生提供了免费的序列号。所以不需要考虑付费的问题。

综上所述、采用IntelliJ IDEA。

> 5.18日更新，观看了浙江大学翁凯教授的Java教程，方知Eclipse软件本身就是用Java编写的，所以要配置好Java环境才能运行这个IDE。这也是为什么网上那么多教程起手就是配环境了。


本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
