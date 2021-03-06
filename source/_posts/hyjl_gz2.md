---
title: GIIC-新一代人工智能产业发展论坛(day2)
date: 2019-11-24 08:58:00
tags:
  - 参会记录
  - Artificaial Intelligence
categories:
  - 参会记录
mathjax:
---
GIIC-第三届全球智能工业大会暨博览会暨全球创新技术成果转移大会
The 3rd Global Intelligent Industry Conference & Exposition & Global Innovative Technology Exchange Conference

主题论坛一： 新一代人工智能产业发展论坛(day2)
主持人：许勇，华南理工大学研究生院教授，副院长

## 7 视觉计算关键技术及应用 The Key Technologies and Applications of Visual Computing
许勇 许勇，华南理工大学研究生院教授，副院长
广东省大数据中心 主任
CCF 广州 主席

90% 的大数据是图像/视频数据

人工智能有哪些类型：
* 弱人工智能
* 通用人工智能
* 强人工智能

讲了一些背景知识，人工智能的发展历史，当前需要解决的问题。

### 图像复原
去躁
去模糊
增强（不清晰到清晰）
视频去模糊
图像恢复（去雾）
去雨
图像恢复（去反光）
去反射

### 图像理解
* 稀疏编码与字典学习
* 大规模场景的人群运动分割
    * 逃跑场景检测
    * 公共场合监控与报警

![视觉对象重检追踪和行为分析](https://i.loli.net/2019/11/26/wmqDzC2YPlkB6xb.png)


![人群逃跑行为检测](https://i.loli.net/2019/11/26/Rwxsf1ASy6MTi9L.jpg)

这里讲的一些应用举例还是比较有意思的，对于人流的分析，可以辅助一些流量决策，比如地铁站的多部扶梯，哪些上行哪些下行，可以根据人群流量特征进行规划调度。
### 视频理解
在哪里？做什么？

同时定位视频中的行为（事件）并进行文字描述
难点：
* 时序行为定位
* 行为描述

算法：双向SST + LSTM

目前困难：缺乏中文视频描述的训练集

应用前景：视频搜索



### 书法评价大数据

一个书法评分的应用

### 介绍课题组其他研究
广东省人民政府大数据科学研究中心项

云脑智能交通分析
> 核心挑战-存储瓶颈，智能瓶颈


## 8 大规模科技知识图谱构建及其在科技情报领域的应用
王德庆， 北京航空航天大学计算机学院特聘研究员，国家科技资源共享服务工程技术研究中心总工

### 传统智库vs新型智库
传统智库，以人为中心，数据为辅，咨询报告的产生往往消耗大量人力物力

新型智库，传统智库的升级版，海量数据为主、人工为辅


### 案例一：知兔——基于科技大数据的智能服务平台
整合：人才、机构、科研成果
提供：科研评价，成果转化
对人才，了解本领域学术热点等等
对企业，找到领域的优秀人、较好的技术

核心竞争力：
* 全
* 准（智能画像）
* 变（更新及时：每个月活跃度指标，时效性高）
* 信（高可信交易）

知兔APP：动态指数评价 + 知识服务，小程序：知兔空间

这个知兔空间小程序还挺有意思，这个知识图谱做的不错，体验了一下，查了查自己的老师，显示的信息还是比较全面的。
### 案例二：科技情报监测预警（科技部）

数据信息

AI分类体系



### 案例三：科技成果转化，产业链图谱

科技->产业界

问题难点：两套语言体系

用图谱的方式显示产业的上下游有何种东西


## 9 视觉与语言结合的研究 Vision  + Language ：From Caption to Grounding
马林，腾讯 AI Lab 计算机视觉中心专家研究员

![](https://i.loli.net/2019/11/26/d1NgpSiKLzDknUs.png)
视觉 非结构化 -> 语言 结构化

Vision|Language
---|---
非机构化|结构化

朋友圈：文字+图片  视觉和语言伴生存在


### Caption
一个图像（视频）： 一个或者多个句子来描述
Encoder-Decoder Structure

产生的语句是 fitting 数据集的，所以


### Grounding
Caption的反任务，给定语句，在视频中找出与之语义一致的片段。检索视频？速度如何？

i-LSTM

SCDM

更复杂场景： Weakly-Supervised Spatio-Temporal Grounding

这是本次会议里面，学术性相对比较高的一个报告。做的的确是深度学习领域的前沿研究。难度也比较大。


## 10 AI赋能空间光学遥感
李维，中国空间技术研究院高级工程师、博士
### 遥感与智能

介绍了一下研究所，研究成果，典型产品
高分二号，高景一号，珠海一号，资源三号多光谱
高分四号，世界上最好的静轨相机

### 传统智能遥感技术
农业、水、森林、城市

城市遥感：数据资源三号，目标房屋、道路、提取，判断政府与土地政策的决策差

* 遥感数据小样本
* 在轨资源受限
* 智能模型鲁棒性查

出乎意料的是，李维博士把当前基于深度学习的遥感技术也作为传统遥感技术，那么看来这个分类标准就是以他们提出的在轨智能遥感技术作为基准，其他的都是传统遥感技术。

他演示了一些基于深度学习的技术结果。和昨天中科院沈阳自动化研究所的老师的结果有很多相似之处，基本上都是现有深度网络模型的一些应用范例。

### 在轨智能遥感技术

利用生成对抗网络来实现样本生成

目标背景合成仿真遥感图样制作

在轨遥感像质状态智能监测补偿技术
![IMG_0839.jpeg](https://i.loli.net/2019/11/26/NWaAnZBcRHSfe1y.jpg)


##11 基于神经网络的新一代人工智能及其在心理学中的应用 The New Generation of Artificaial Intelligence Based on Neural Networks and Using that in Psychology
Ardavan Karbasi, 深圳智慧林网络科技有限公司-美籍专家、技术总监、博士

英文报告，内容比较由浅入深
主要是一些情绪发现的内容


##12 AI Core 赋能场景应用
刘梦涛，广州广电运通金融电子股份有限公司研究员副总工程师

从企业角度、行业解决方案应用角度

算法如何接入数据、算法如何应用行业

业务 -> 技术 -> 数据  三者如何连接

* 从 ATM 到 AI
戴口罩、墨镜取款行为的识别，是否有人在偷瞄这种识别。

* 银行网点内的监测

* 智能安检一体机

* 智慧园区  可以视为一个放大的银行网点

* 未来酒店

这位讲者是一名企业家，主要讲了一些他们公司做的内容。一些应用。其中说的是，客户不关心技术细节，需要你提供全套服务。
