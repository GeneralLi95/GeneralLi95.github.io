---
title: MySQL安装与初始化
date: 2018-06-05 12:51:38
tags:
  - MySQL
  - 数据库
categories: 数据库
catalog: true
---

## 安装

使用brew进行安装

```
brew install mysql
```

安装好之后通常会没有linked
然后

```
sudo chown R ‘usrname’ usr/local

```

通常会执行成功，但是macOS比较新的版本里面会没有拒绝这一操作。即Operation not permitted。这是由于Mac的rootless机制。

解决方案：

1. 重启Mac，重启时按住command + R 进入恢复模式。（话说还是第一次知道Mac还有这个模式）

2. 选择终端，在左上角，输入指令

  ```
  csrutil disable
  ```

3. 再重启让机器正常启动，可以在终端查看rootless状态：

  ```
  csrutil status
  ```
  正常情况下rootless已经关闭了

4. 要想重新开启，参照步骤1，2


## 设置root密码

终端输入

```
mysqladmin -u root password 123456
```
就已经把密码设置成123456了。
然后登陆

```
mysql -u root -p
```

输入 123456.即可

## 启动和关闭MySQL服务器

通过命令来检查MySQL服务器是否启动：

```
ps -ef | grep mysqld
```
 如果MySQL启动的情况下，将输出MySQL的进程列表。

 如果要关闭目前运行的MySQL服务器，可以执行下面命令来关闭服务器：

```
mysqladmin -u root -p shutdown
```

启动MySQL服务器

```
mysql.server start
```


## 管理MySQL的命令
列出数据库列表:


```
mysql > SHOW DATABASES;
```

会显示当前的已有的数据库列表：

默认的数据库Database有
information_schema
mysql
performance_schema
sys
这四个
然后从 databese列表里面挑一个

```
mysql > use mysql;
```

这时候切换到database mysql里面。然后可以用

```
mysql > SHOW TABLES;
```

看一下这个数据库里面有哪几个表

然后可以看一下一个表中有那几个列（比如从表db中查看）

```
mysql > SHOW COLUMNS FROM db;
```

从这几行中我们可以搞清楚这几个概念的从属关系。 DATABASE -> TABLES -> COLUMNS


***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
