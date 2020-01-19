---
title: pandas 学习
date: 2020-01-09 23:45:56
tags:
  - Python
  - Pandas
categories:
  - Data Mining
mathjax:
---

Pandas

df dataframe 的简写

## 采坑记录

pandas-lzma-error

解决方法：
https://github.com/pandas-dev/pandas/issues/27532
在 macOS 上
* `pip freeze > requirement.txt`
* `pyenv uninstall 3.7.6`
* `brew install xz(This is how you pick up the correct lzma macOS)`
* pyenv install 3.7.6

进行快速测试

* `pip install pandas` 然后运行检测到其正常工作.
* `pip install -r requirements.txt`
