---
title: Atom 包管理器 apm 更换国内源
date: 2020-02-18 12:12:36
tags:
  - Atom
categories:
  - Software
mathjax:
---
今日发现 Atom 更新包无论如何总是失败。报错类型是下面这样

```
npm ERR! code Z_BUF_ERROR
npm ERR! errno -5
npm ERR! zlib: unexpected end of file

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/yaoli/.atom/.apm/_logs/2020-02-17T09_39_31_164Z-debug.log
```

尝试了清除 npm cache 重装 发现并没有用，设置代理也一直未成功，另外由于梯子的不稳定性，设置代理后如果梯子挂掉了，仍然是不能用。

所以最后查阅资料，采用了更换淘宝源的办法。

> Atom的插件实际上在npm上，npm的官方源在国内访问起来是非常缓慢的。但是，国内有许多镜像源可以使用，如淘宝源(http://registry.npm.taobao.org/)，CNPM（http://r.cnpmjs.org）

执行如下命令

```
apm config set registry http://registry.npm.taobao.org
```

即完成了对 npm 源的替换。Atom 终于可以正常使用了。
