---
title: GitHub 头像无法显示的问题
date: 2020-02-18 17:49:12
tags:
  - GitHub
  - Proxy
categories:
  - Software
mathjax:
---
发现 GitHub 的头像一直无法显示，包括自己的和他人的。虽然这看似是一个无关紧要的问题，但是会影响 网页浏览 和 Atom git 界面的美观，逼死强迫症。

查阅资料发现，github 的头像存储在其分服务器下面，所以单单将 github.com 加到 PAC 规则里面是不够的，还需要将其他一些服务器也加进去。

目前我的 PAC 列表如下：

```
! Put user rules line by line in this file.
! See https://adblockplus.org/en/filter-cheatsheet ||coursera.org
||github.com
||atom.io
||raw.githubusercontent.com
||avatars0.githubusercontent.com
||avatars1.githubusercontent.com
||avatars2.githubusercontent.com
||avatars3.githubusercontent.com
||avatars4.githubusercontent.com
||avatars5.githubusercontent.com
||avatars6.githubusercontent.com

```

其中后面带 githubusercontent 的即存储头像的地址。
