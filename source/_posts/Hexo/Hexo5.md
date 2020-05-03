---
title: Hexo博客搭建（五）SEO优化
date: 2018-04-02 20:29:45
tags: Hexo
categories: Hexo
---
# 与GitHub仓库互联
直到目前，我们的blog还只是一个本地的小家伙，像一个没有出过门的孩子，只能在localhost:4000里面查看。别人是看不到的。那么如何把它部署到线上？

一般建网站的都需要租服务器，买VPS，亚马逊，阿里云等等，还要花一笔小钱。

好在我们有GitHub。

GitHub提供里代码托管服务，作为全世界最大的程序员社区，自然不缺乏脑洞清奇的人，很多人在上面做各种各样的事情，这是后话。

如果是首次使用GitHub，配置过程是比较复杂的，可先看一下 [廖雪峰的git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。

假定我们已经配好了环境，那么就很容易了。

在GitHub上面新建一个repo，注意项目名称为  "用户名.github.io"否则之后是无法访问的，一个账户只能建一个github pages。之后将之前的public文件夹里的内容都同步到这个项目的master分支，之后浏览器访问用户名.github.io就能看到hexo的博客界面了。

比如我的repo 名字是
![image](http://upload-images.jianshu.io/upload_images/11400909-2489f8b1cadd7dc0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
那我在浏览器地址栏输入 generalli95.github.io就可以访问我的网页了！

使用git 命令去push需要先hexo generate，还是略显麻烦，hexo提供了一个插件hexo-deployer-git可以打包git命令。

插件安装,命令行先cd到blog，然后输入下面命令。
```
npm install hexo-deployer-git --save
```
然后在博客的配置文件_config.yml，添加
![image](http://upload-images.jianshu.io/upload_images/11400909-4bab54cc5b099520.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<font color=red>注意：把repo:后面换成自己的repo名字！</font>

然后hexo generate之后，再hexo deploy就可以自动部署了，支持短命令，hexo g -d,相当于前两个命令的合体！

# 换上自己的域名
得益于当年的中二岁月……申请github的时候非要起个什么英文名字，然后……自己都打不对自己的网页全名，因为实在是太长了？

所以，要不自己注册个域名，这样显得很酷炫，而且说不定哪天域名还能卖个好价钱，想想还真得好激动呢（白日梦）。

## 购买域名
购买域名的具体操作。推荐先看一下这篇文章： [推荐几家域名注册服务商](https://zhuanlan.zhihu.com/p/27349039)。
我是在Godaddy上注册的域名，网站有中文版支持支付宝支付还是很友好的。域名前两年有活动，前两年109块，后面每年100多一点，价格不贵。并且这可能也是我们这个博客搭建过程中唯一需要花钱的地方。

## 设置DNS解析
实际上这也不是一个必须的服务，但是由于Godaddy是一个国外厂商，直接使用它的DNS速度有影响，所以为了保证域名在国内的解析速度。推荐使用DNSPod的DNS解析服务。

<font color=red>DNSPod</font>已经被腾讯云收购，所以用微信可以直接登录。
登录DNSPod之后按照提示，再到Godaddy里面把DNS修改一下。
![image](http://upload-images.jianshu.io/upload_images/11400909-1c39056181935b67.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后为了使GitHub接收这个域名，还需要博客的根目录下添加一个名为CNAME的文件（不要加.txt）。
这个文件放到主题文件夹的source里面，文件里面放你的域名（去掉www），比如我的网站，文件里面就放一句话：liyaolife.com
然后在如上图所示界面里面，添加两条记录，一个主机记录写@，另一个写www，这样无论用户输入www.liyaolife.com 还是只输入 liyaolife.com 都可以直接定位到我的网站了，记录值放自己的GitHub Pages地址。

OK，现在可以把这个URL转到微信群里面跟爸爸妈妈还有小伙伴们炫耀一番了。

***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
