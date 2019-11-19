---
title: MySQL 基础指令
date: 2019-11-18 08:55:18
tags:
  - MySQL
  - 数据库
categories:
  - 数据库
mathjax:
---

## 启动和关闭 MySQL 服务器

启动
```
mysql.server start
```

关闭服务器（需要输入密码）

```
mysqladmin -u root -p shutdown
```

查看服务器是否启动

```
ps -ef | grep mysqld
```

## 管理 MySQL 服务器

登录服务器（前提是已经启动）
```
mysql -u root -p
```

登出服务器

```
quit
```

设置 root 密码 ：
终端输入

```
mysqladmin -u root password 123456
```
就已经把密码设置成123456了

## 基础操作指令

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
