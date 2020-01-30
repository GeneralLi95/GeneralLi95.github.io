---
title: 2020天池智慧海洋建设比赛
date: 2020-01-10 10:14:40
tags:
  - 天池
categories:
  - Data Mining
mathjax:
---
## 赛题背景

>基于位置数据对海上目标进行智能识别和作业行为分析，要求选手通过分析渔船北斗设备位置数据，得出该船的生产作业行为，具体判断出是拖网作业、围网作业还是流刺网作业。

[竞赛官网](https://tianchi.aliyun.com/competition/entrance/231768/introduction)

捕鱼方法: wiki: [Fishing Techniques](https://en.wikipedia.org/wiki/Fishing_techniques)
* 拖网作业 wiki [Trawling](https://en.wikipedia.org/wiki/Trawling) 也称 Dragging
* 围网作业 wiki: [Seine fishning](https://en.wikipedia.org/wiki/Seine_fishing) 也称 Purse seine 或Seineingg
* 流刺网作业 wiki: [gillnetting](https://en.wikipedia.org/wiki/Gillnetting)

![Salmon-Gillnetting.jpg](https://i.loli.net/2020/01/10/XPpt5yvYMzenHOw.jpg)




参考文献
1. [Quality and status of fish stocks in lakes: gillnetting, seining, trawling and hydroacoustics as sampling methods](https://link.springer.com/article/10.1007/s10750-010-0385-6)
2. [Fisheries Techniques: Second edition](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=Fisheries+Techniques%3A+Second+edition&btnG=)
3. [Gillnetting – how it works](https://skipperotto.com/gillnetting-how-it-works/)
4. [Finfish Capture Techniques](https://www.montereyfish.com/finfish-techniques) 详细介绍了鳍雨捕鱼作业的各种技术手段，不仅是题目列举的两个，还有其他几种，图片比较多，写论文 的 background 的时候可以参考。

## 数据

> 初赛提供 11000 条渔船北斗数据，数据包含脱敏后的渔船ID、经纬度坐标、上报时间、速度、航向信息，由于真实场景下海上环境复杂，经常出现信号丢失，设备故障灯原因导致的上报坐标错误、上报数据丢失、甚至有些设备疯狂上报等。

数据示例：

渔船ID | x | y | 速度|方向|time|type
---|---|---|---|---|---|---
1102|6283649.656204367|5284013.963699763|3|12.1|0921 09:00|围网

渔船ID：渔船的唯一识别，结果文件以此ID为标示
x: 渔船在平面坐标系的x轴坐标
y: 渔船在平面坐标系的y轴坐标
速度：渔船当前时刻航速，单位节
方向：渔船当前时刻航首向，单位度
time：数据上报时刻，单位月日 时：分
type：渔船label，作业类型


## 论文草稿

Fishing Techniques Classification based on Beidou Tracking and Machine Learning

北斗导航信息广泛应用于中国渔业。在海洋上，渔民无法通过蜂窝移动网络来进行定位与传递信息。因此，中国政府要求所有渔船安装北斗系统以此来对他们进行管理和保护。海洋捕鱼方式经过长期的发展，有很多种，比如a,b,c。最常见的是某三种。不同的捕鱼技术有其各自的原理，特点和适用场景。
Fishermen can't locate themselves and communication with the land through cellular mobile networks. So Satellite positioning navigation system

After long-term development, there are many types of marine fishing methods, such as a, b, and c. The most common are some three. Different fishing techniques have their own principles, characteristics and application scenarios

在本文中，我们通过对渔船的轨迹信息的挖掘，分析出其捕鱼作业的方式。


## 参考文献
1. Li, Xingxing, et al. "Precise positioning with current multi-constellation global navigation satellite systems: GPS, GLONASS, Galileo and BeiDou." Scientific reports 5 (2015): 8328.
2. Simmonds, John, and David N. MacLennan. Fisheries acoustics: theory and practice. John Wiley & Sons, 2008.
