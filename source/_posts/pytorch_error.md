---
title: Pytorch THCudaCheck FAIL 错误
date: 2019-12-26 10:42:17
tags:
  - PyTorch
categories:
  - 机器学习
mathjax:
---

在服务器 (GTX 2080 Ti) 上跑代码的时候出现错误
```
THCudaCheck FAIL file=/pytorch/aten/src/THC/THCGeneral.cpp line=405 error=11 : invalid argument
```
经查 发现是 cudnn.benchmark = True 时发生的，将 torch 更新到 1.0 以上版本后解决问题。这个问题并不常见，因为和 CUDA 的版本与 torch 的版本共同相关，另外 cudnn.benchmark 默认是 Flase。所以隐藏的很深。

关于 cudnn.benchmark 的作用可以参考这个 [知乎:torch.backends.cudnn.benchmark ?!](https://zhuanlan.zhihu.com/p/73711222)，关于这个bug的更多解释可以看这个 [A error when using GPU](https://discuss.pytorch.org/t/a-error-when-using-gpu/32761)
