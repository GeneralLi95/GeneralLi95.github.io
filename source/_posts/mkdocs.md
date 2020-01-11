---
title: mkdocs 使用
date: 2020-01-11 10:59:12
tags:
  - mkdocs
categories:
  - Python
mathjax:
---
mkdoc 基于 Python 的轻量级的网站生成利器，可以用来做手册、博客、等等。


```
pip install mkdocs  # 基础包
pip install mkdocs-material   #  主题
pip install pymdown-extensions  # 拓展，支持数学公式和emoji表情
```
本地生成预览

```
mkdocs serve
```

GitHub Pages 部署

```
mkdocs gh-deploy
```
