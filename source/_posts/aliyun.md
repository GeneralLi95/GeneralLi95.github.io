---
title: 阿里云远程深度学习服务器使用记录
date: 2020-05-03 10:09:20
tags:
  - Deep Learning
  - Aliyun
categories:
  - Machine Learning
  - Deep Learning
mathjax:
---

## 常用指令记录：

查看正在运行的 python 程序
```
ps -ef | grep python

```

查看 GPU 状态

```
nvidia-smi
```
回显解释

> GPU Fan 风扇转速 从 0-100% 之间变动，有的设备不会返回转速，阿里云服务器即没有返回转速，应该这些线上服务器都是有单独的散热系统
> Temp ： 温度，摄氏度
> Perf : performance 简写，性能状态，从 P0 到 P12， P0 表示最高性能
> Pwr : 能耗，单位是瓦
> Bus-ID : 与 GPU 总线相关
> Dispaly Active 表示 GPU 的显示是否初始化
> Memory-Usage: 显存使用率
> Volatile GPU-Util ： GPU 利用率


## 深度学习

### 使用 localhost 远程访问 tensorboard

解决办法

1. ssh 连接时，将服务器的6006端口重定向到自己的机器上来：

```
ssh -L 16006:127.0.0.1:6006 username@remote_server_ip
```

其中 16006:127.0.0.1 代表自己机器上的 16006 号端口，6006是服务器上的Tensorboard使用端口

2. 在服务器上使用 6006 端口（默认即为6006） 正常启动 TensorBoard:
```
tensorboard --logdir MODEL_DIR
```
3. 在本地服务器中输入地址： `localhost:16006` 即可

[参考](https://stackoverflow.com/questions/37987839/how-can-i-run-tensorboard-on-a-remote-server)

## Pycharm remote development
使用 PyCharm
上传：
