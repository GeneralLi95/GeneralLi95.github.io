---
title: Python 虚拟环境
date: 2019-12-25 23:25:16
tags:
  - Python
categories:
  - Python
mathjax:
---
以前在自己的 mac 笔记本跑代码一直用的一套环境，什么都往里面装，基本上没用过虚拟环境。最近在服务器上跑代码，因为多人共用，自己需要更新软件包，又不能影响别人，所以新建虚拟环境是最好的选择。

## 管理虚拟环境

### 使用 virtualenv
先安装 virtualenv
```
$ pip install virtualenv
```

为某个项目建立虚拟环境
```
$ cd project_folder
$ virtualenv venv
```

启动虚拟环境
```
$ source venv/bin/activate
```
关闭虚拟环境
```
$ deactivate
```

导出 requirements
```
$ pip freeze > requirements.txt
```

根据 requirements 安装 packages
```
$ pip install -r requirements.txt
```
### 使用 virtualenv-clone 来 copy 一个虚拟环境

```
$ pip install virtualenv-clone
$ virtualenv-clone venvA venvB
```
这里 venvA 是源，venvB是目标

在操作过程中，我发现，copy过去的虚拟环境，文件夹名字是不一样了，但是启动进去后命令行最前面的括弧里的小名字还是和原来的一样，需要手动改一下 bin/activate 里面的这一块描述，总之不麻烦。

### 使用 virtualenvwrapper
virtualenvwrapper 功能等于 virtualenv + virtualenv-clone  从建立虚拟环境，到克隆都有一系列命令，这里列出一个克隆命令
```
$ cpvirtualenv ENVNAME [TARGETENVNAME]
```

### 使用 Anaconda 管理虚拟环境
前面的三个包都是在 pip 情况下的虚拟环境管理方法，使用conda的话是另外一套体系了。(conda 的并列级别是 pip

## 参考资料
1. [The Hitchhiker's Guide to Python:Pipenv & Virtual Environments](https://docs.python-guide.org/dev/virtualenvs/)
2. [stackExchange:Create a copy of virtualenv locally without pip install](https://askubuntu.com/questions/737098/create-a-copy-of-virtualenv-locally-without-pip-install)
3. [stackoverflow:How to duplicate virtualenv](https://stackoverflow.com/questions/7438681/how-to-duplicate-virtualenv)
