---
title: zsh 初探
date: 2020-01-08 15:23:44
tags:
  - Shell
categories:
  - Shell
mathjax:
---

今天才发现不知道什么时候我的 shell 环境莫名其妙变成了 zsh。

今天了解了一下 zsh，也叫 Z shell 是一种将 macOS 原生终端的 bash 命令行打包起来，然后增加了很多新的不错的特性的命令行。

直观体会是 首先简洁，以前的我命令行起手是

```
YaodeMacBook-Pro: ~ $
```

现在起手变成了(现在发现也不是每次打开都这样)

```
yaoli@YaodeMBP: ~ $
```

其他好处就是

* 完全兼容 Bash
* 更强大的补全功能 cd 之后可以按多次 tab 键，而不用按 ls 键来显示所有的内容，另外可以智能切换目录比如你要进入一个很深的目录，`/var/log/nginx/error/lastyear/may/first/monday` 用zsh可以这样输入`cd /v/l/n/e/l/m/f/m`
* 命令选项补齐，比如输入 docker，按 tab 会显示出 docker 都有哪些命令选项。
* 大小写自动更正
* 各种各样丰富多彩的主题   


## 参考资料

1. [What is ZSH, and Why Should You Use It Instead of Bash?](https://www.howtogeek.com/362409/what-is-zsh-and-why-should-you-use-it-instead-of-bash/)
2. [mac 装了 oh my zsh 后比用 bash 具体好在哪儿？](https://www.zhihu.com/question/29977255)
