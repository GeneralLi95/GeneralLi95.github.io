---
title: Mac 设置终端代理
date: 2020-02-16 21:30:28
tags:
  - Shell
  - Proxy
categories:
  - Shell
mathjax:
---

在配置科学上网的时候，无论 PAC 模式还是还是全局模式，在终端都是不适用的。终端需要单独设置代理(类似 Telegram，或者 Foxamail 里面为某个邮箱单独设置代理)。

方法，修改 .bash_profile ，如果 bash 已经更新为 zsh 的话，修改 .zshrc 文件

在其中加入

```
# http://127.0.0.1:1080 替换成自己的 IP 和端口
alias goproxy='export http_proxy=http://127.0.0.1:1080 https_proxy=http://127.0.0.1:1080'
alias disproxy='unset http_proxy https_proxy'
```

注意是 http 协议代理端口，不是 socks 端口。编辑完成后保存退出。

使用
```
source ~/.zshrc
```
或者重启电脑，使新的配置文件生效。接下来使用方法

```
# 开启代理
% setproxy    
HTTP Proxy on
# 关闭代理
% unsetproxy  
HTTP Proxy off
```
不过，发现即使设置代理之后 ping google.com 还是不行，查了资料之后发现，ping 命令走的是 icmp 协议，http 是应用层协议，而 icmp 是网络层协议。配置的应用层协议不会影响网络层协议，所以即使配置代理之后还是 ping 不通 Google。需要安装 httping 工具。不再赘述。
