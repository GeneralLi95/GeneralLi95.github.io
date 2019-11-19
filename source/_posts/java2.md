---
title: Java HelloWorld项目创建
date: 2018-05-19 13:42:39
tags:
  - Java
  - 基础编程
categories: Java
---
# HelloWorld项目创建

以前学Python的时候大家都说Python简单，当时没什么感觉，如今回头来学Java才发现，Python确实简单。Python的HelloWord只要在console里面一行代码就解决了。Java就很麻烦。

## Creat New Project
选择Java EE，然后Next。之后又会有从模板创建的选项(Create project from template)，此选项下有两个选择Command Line App 和Java Hello world。前者会自动创建一个带有main方法的类，后者会自动穿件一个带有main方法的并且会打印输出Hello World的类。

不勾选此选项，从零开始。

* src目录右键，new packages 创建新的包目录

* 在包下面创建class,选择创建class，文件名比如是hello.则生成的文件即为hello.java(原来class文件的后缀名是.java)。然后会自动生成一部分代码，包括
*
    ```java
    public class hello{
    }
    ```
的声明。

起初此处我是直接新建一个file，然后自己命名的hello.java，手敲的public class 声明，结果不能运行，发现问题是手敲的public class 声明的类名与hello.java文件名不一致。


之后

```java
package hellopack;

public class HelloWorld{
    public static void main(String[] args){
        System.out.print("Hello world");
    }
}
```
即可输出Hello world了。


***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
