---
title: torch.cuda.is_available() False 问题
date: 2020-01-06 16:20:57
tags:
  - PyTorch
categories:
  - 机器学习
mathjax:
---
为了解决前文中提到的 cudnn.benchmark = True 的情况下的报错，我在实验室的两台新的服务器都新建路虚拟环境，升级了 torch 版本到1.3.1，升级后在61服务器可以正常使用，在57服务器，该虚拟环境中无法正常使用GPU，运行时  device 显示的是 'cpu'。

具体表现为

```shell
>>> import torch
>>> torch.backends.cudnn.enabled
True
>>> torch.cuda.is_available()
False
```

Google 检索关键字 torch.cuda.is_available = false
发现遇到这个问题的人不少，很多种解法，做玄乎的是重启服务器就好，大部分说法是更新 CUDA 的版本和 NVIDIA Driver 版本，而受限于网络，离线环境下这两个版本我是不敢轻易更新的，之前服务器安装的时候，联网都要折腾好几天。而这个时候，考虑到服务器还有很多人在用，这个也不像虚拟环境那样可以轻松安装，所以暂时先不考虑，目前的折中做法只能是只在 61服务器上用 或者在57服务器上将 cudnn.benchmark 设置成 false ，有时候科研当中也是充满了无奈啊，不过时间已经不允许在这些上面浪费太多了。
