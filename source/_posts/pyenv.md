---
title: 怎样在 Mac 上安装一个最好的 Python 工作环境
date: 2020-01-09 09:56:07
tags:
  - Python
categories:
  - Python
mathjax:
---
Python 环境的布置真的是个大难题，mac 内置 python2.7，可以通过 brew 安装 python3，也可以在官网原生安装 python3，有的是用 anaconda 一套全搞定，有的是用 pip 来管理，总之电脑上的 python 环境就像一条大蛇，很容易失控。就像下面这张图画的，在不同位置乱成一锅粥，也会引起一些冲突。
![image.png](https://i.loli.net/2020/01/09/F9cHlR57tbuwymN.png)

前几天修复了 一个 pytorch 不能自动下载 checkpoints 的问题，然后今天就发现 jupyter notebook 、 mkdocs 等命令都不管用了。今天下定决心开始重新布置本机的Python环境。

## 清除以往的Python版本
发现本机存在 3个 Python版本，Python官网下载的 3.6.5 版本，系统自带的2.7版本以及通过 brew 安装的 2.7版本。以及残存的通过 brew 安装3.6.5和3.7版本经过删除后的残余（大概几十KB）

首先通过 `brew uninstall` 删除了所有通过 brew 安装的版本。比较方便。

然后开始清理在官网下载的 3.6.5 版本。
1. 在应用程序删除 Python APP整个文件夹
2. 在 资源库/FrameWorks/ 里面删除 Python.frameworks
3. 用 磁盘清理软件清理

## 正确的方式安装 Python 版本

> "The basic premise of all Python development is to never use the system Python. You do not want the Mac OS X 'default Python' to be 'python3.' You want to never care about default Python."——Moshe Zadka


```
% brew install pyenv
🍺  /usr/local/Cellar/pyenv/1.2.16: 671 files, 2.5MB
```

```
% pyenv install 3.8.1
python-build: use openssl@1.1 from homebrew
python-build: use readline from homebrew
Downloading Python-3.8.1.tar.xz...
-> https://www.python.org/ftp/python/3.8.1/Python-3.8.1.tar.xz
Installing Python-3.8.1...
python-build: use readline from homebrew
python-build: use zlib from xcode sdk
Installed Python-3.8.1 to /Users/yaoli/.pyenv/versions/3.8.1
```

还需要将 pyenv 加入到命令行设置文件里，原生命令行是 **.bash_profile**，zsh是 **.zshrc**
>The power of pyenv comes from its control over our shell's path. In order for it to work correctly, we need to add the following to our configuration file (.zshrc for me, possibly .bash_profile for you):



```
pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

然后

```
% pyenv global 3.8.1
% pyenv version
3.8.1 (set by /Users/yaoli/.pyenv/version)
```

重启 terminal 或者 `exec $Shell`

```
% pyenv versions
system
* 3.8.1 (set by /Users/yaoli/.pyenv/version)

% which python
/Users/yaoli/.pyenv/shims/python

% python -V
Python 3.8.1

% pip -V
ip 19.2.3 from /Users/yaoli/.pyenv/versions/3.8.1/lib/python3.8/site-packages/pip (python 3.8)
```


大功告成！

---
2020.1.11 发现 Python 3.8.1 目前还不支持 PyTorch，谨慎升级。我已经回退到 3.7.6。





## 参考资料

1. [The right and wrong way to set Python 3 as default on a Mac](https://opensource.com/article/19/5/python-3-default-mac)

2. [Python Environment](https://xkcd.com/1987/)

3. [Managing Multiple Python Versions With pyenv](https://realpython.com/intro-to-pyenv/)
