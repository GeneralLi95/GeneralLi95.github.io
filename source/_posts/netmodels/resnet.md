---
title: ResNet 笔记
date: 2019-12-17 08:57:34
tags:
  - 机器学习
  - 深度学习
  - 神经网络
categories:
  - 机器学习
mathjax: True
---
## ResNet
论文提出  何恺明(微软研究院)   2015
[Deep residual learning for image recognition](http://openaccess.thecvf.com/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf)


### 批标准化层 BN
Batch Normalization Google2015年提出
[Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/pdf/1502.03167v3.pdf)
**为何需要 BN**
解决 *Internal Covariate Shift(ICS)* 问题。在统计及其学习中的一个经典假设是”源空间(source domain)和目标空间(target domain)的数据分布(distribution)是一致的。如果不一致就会出席一些新的问题，比如 transfer learning/domain adaptation等，而 covariate shift 就是分布不一致假设之下的一个分支问题，它是指源空间和目标攻坚的条件概率是一致的，但是其边缘概率不同。即对所有的 $x \in \mathcal{X}, P_s(Y|X = x)=P_t(Y|X=x)$ 但是 $P_s(X) \neq P_t(X)$。显然对于神经网络的各层输出，由于经过了层内的操作，其分布与各层对应的输入信号分布不同，而且差异会随着网络深度增大而增大，可是他们所能“指示”的标注样本(label)仍然是不变的，所以这就符合 covariate shift 的定义。由于是对层间信号的分析，所以也是 "internal"。


![BN操作(从论文中截图)](https://i.loli.net/2019/12/17/4Bo1XFyHfLwcelW.png)
