---
title: Hexo博客搭建（二）从最简单的开始
date: 2018-03-27 11:16:36
tags: Hexo
catalog: true
---


## 安装git, node, hexo
macOS和Linux中都自带git，使用macOS的强烈建议安装brew，之后使用brew命令安装各种东西都很方便。

假定现在brew和git已经装好了。下面安装node，打开终端

```shell
 $ brew install node
```
OK，node装好了，之后使用node自带的包管理器(node package management?)安装hexo。

```shell
$ npm install hexo-cli -g
```
如果需要权限的话，在前面加上sudo即可。

OK现在开始新建一个blog。
先新建一个文件夹，姑且就命名为blog吧，当然你也可以命名其他名字，终端cd到这个文件夹下面。
下面的命令都在blog文件夹里执行。

```shell
$ hexo init
$ npm install
$ hexo server
```
解释一下这几个命令，hexo init 和git init一样，（init initialization缩写，初始化）使这个blog文件夹成为一个“黑箱",只有在这里面”hexo“的相关指令才有效，npm install安装相关文件，hexo server，启动本地服务器（默认端口4000），现在打开浏览器，在地址栏输入，localhost:4000。OK一个博客雏形就好了。hexo server  命令可以简写为hexo s。

最开始的界面大概就长这个样子：
![image](http://upload-images.jianshu.io/upload_images/11400909-abee9df147ccfa44.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


好像还蛮容易的。
参考：[shino的博客](https://shino.space/2016/使用Hexo在GitHub上搭建网站/) ，[Hexo官网](https://hexo.io/zh-cn/)

## 博文写作

```shell
$ hexo new test
```
通过这个命令在，blog文件夹下面就有了一个test.md文件。.md表明这是一个Markdown格式的文档，注意如果文档名包含空格的话注意加双引号。

如果要编辑这个文档，使用任何文本编辑器一颗，有一些专用的Markdown编辑器，比如Mou，Macdown，以及我目前用的MWeb（强烈推荐），一些IDE也可以加装Markdown插件，比如Pycharm，Webstorm。当然目前很多高手喜欢用Github出品的号称21世纪的文本编辑器Atom。

总之不管使用什么编辑器，Markdown的语法都是一样的。
使用Markdown编辑好，保存。重新hexo s 即可刷新本地的网页。


## 使用MWeb与七牛云图床优化写作体验
```
该部分不是必须的，可以跳过。
```
MWeb作为Mac APP store里面非常优秀的Markdown编辑器。使用MWeb，具体方法，打开MWeb之后Command+E，进入外部模式，跟IDE打开project一样打开blog/soure即可，本地编辑，保存，命令行一键发布，简直不能再爽了。

<font color=red>图床是什么鬼，为什么要使用图床？</font>
当我们在本地做一个Word或者PPT文档的时候，图片的插入非常方便，只要拖进去就行了。

但是也存在一个问题，插入图片越多，文档就越大，打开时间也就越长，一台老机子开一个Word有时候需要30s以上，网页虽然道理不太一样，但是大体也是图片越多，打开越慢。

HTML语言里面，图片都是作为标签引用。有过公众号运营的同学可能有体会，插入图片需要先上传到公众号相册，然后再在相册里面选一下。非常麻烦。那能否直接插入图片？

答案必须是Yes。

这时候图床就派上用场了，拖进去，上传，复制Markdown，这时候你的文档里面是一个
```
![](http://p6atp7tts.bkt.clouddn.com/15222405680070.jpg)
```
这样的东西，相当于图片变成了一个源。

Hexo博客其实本身不存在这个问题，因为拖进去的图片会被保存下来，一起上传到GitHub repo里面，相当于自动完成了这个过程。但是这样博客的迁移性就不太好，如果要随便迁移，比如要迁移到微信公众号或CSDN，这个方法就比较有用了。关于MWeb和七牛的配置具体可见下面两个资源。
[MWeb](https://blog.csdn.net/crazy1235/article/details/78570367) ，[七牛云图床](https://www.cnblogs.com/colder219/p/6266296.html)

还有一篇文章，介绍的比较详细，可以看出MWeb+七牛到底有什么好处。
[一个码子工作者的正确书写发文姿势](https://www.jianshu.com/p/c859ead1b493)
***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
