---
title: 博客最新设置
date: 2020-02-18 22:24:15
tags:
  - Hexo
categories:
  - Hexo
mathjax:
---

一直觉得 Next 主题的博客首页内容过多，显示了文章所有内容显得有些太长，所以今天更新，博客首页设置为 Archive 内容。即归档。

设置办法：

1. 将 Next 主题页 index.swig 内容由 archive 替代。为了保存原来内容，将 原来 index.swig 文件另存为 inde_old.swig。
2. 在博客根目录 `_config.yml` 中将 `index_generater` 下 per_page 内容改为 25，这样首页即展示 25 个博文的标题

其他内容：
1. 增加了 License，修复了百度与 Google 统计。
2. Sidebar 新增「友邻」，设置方法：在 Next 主题 `_config.yml` Menu 下新增 「友邻」，指定其位置位于博客根目录 `source\friend`，指定其图标为 `link`，在指定位置 新建 `index.md`，即可，此处这个文档的 font-matter 不能和普通 post 一样，否则会出现一些错误。
