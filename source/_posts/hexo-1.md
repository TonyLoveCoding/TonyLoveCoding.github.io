---
title: Hexo系列（一）搭建Hexo博客
categories: 教程
tags: Hexo
abbrlink: 7139
date: 2018-04-02 20:50:55
---
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

附上官方中文文档：[Hexo官方文档](https://hexo.io/zh-cn/docs/)

## 基本环境

建立一个基于Hexo博客框架需要有以下环境（没有的话自行查看官方文档的安装教程）:

* Git

* Node.js

* GitHub帐号

## 安装Hexo
在你喜欢的目录文件夹下，右键后菜单中有个"Git Bash Here"选项，点击打开终端输入以下命令

	$ npm install -g hexo
等安装后，输入

	$ hexo init
安装依赖包：

	$ npm install
现在可以输入hexo的常用命令来看看hexo初步模样

	$ hexo g //generate 生成静态文件的命令
	$ hexo s //server 启动并运行在本地服务器的命令
然后访问[localhost:4000](localhost:4000)就成功看到Hexo了。
但现在这个博客只是在你的本地上运行，要想部署在网络上需要用到GitHub。

## 部署Hexo到Github上——配置文件
在Github中创建一个仓库（即项目）

![建立仓库](/img/hexo_1_1.png)

**（注意：仓库名字必须是你的Github账户名字.github.io）**

然后用[Notopad++](https://notepad-plus-plus.org/)（一个很强大很强大很强大的文本编辑软件）打开hexo根目录下的_config.yml文件（最好先备份一个）
拉到文件最下面，找到deploy节点，配置你hexo博客部署的网站。

	deploy:
	  type: git
	  repo:
	      github: git@github.com:TonyLoveCoding/TonyLoveCoding.github.io.git,master
请注意以下几点：

* **配置时把这段代码的TonyLoveCoding换成你自己的Github账号名称。**

* **Hexo的配置文件中所有冒号后面必须紧跟着一个空格。**

* **对于新版本的Hexo而言，配置文件中每行的缩进是区别节点等级的标识，不要随意输入tab符号。比如depoly前面的-号就代表它有两个子节点type和repo，repo又有子节点github等等。**
![配置](/img/hexo_1_2.png)

## 部署Hexo到Github上——配置SSH
GitBash输入指令：

	ls -al ~/.ssh
如果你是Github老用户应该会存在SSH文件，如果没有就继续

	ssh-keygen -t rsa -C "765534395@qq.com"
记得换成你Github的绑定邮箱，遇到输入时直接回车就行。然后以下命令：

	ssh-agent -s
	ssh-add ~/.ssh/id_rsa
如果出错，试试

	eval `ssh-agent -s`
	ssh-add
成功了后，在C:/Users/你的用户名称/.ssh目录下就应该能看到id_rsa.pub了
![SSH](/img/hexo_1_3.png)
用Notepad++打开并复制里面全部内容。到GitHub中Setting中进行如下操作：
![填写SSH](/img/hexo_1_4.png)
这样就完成了配置SSH，接着测试一下SSH

	ssh -T git@github.com

有输入的话就yes。现在让我们开始部署到GitHub上。先安装hexo-deployer-git模块

	npm install hexo-deployer-git --save
然后开始部署：

	hexo g //generate 生成静态文件
	hexo d //deploy 部署到配置文件中指定的地点

现在就可以访问你的博客了，地址是：你的GitHub名称.github.io
## 发布文章
	hexo new "test_new_artcle"
然后在\hexo\source\_posts下就有了test_new_artcle.md文件了。
由于Hexo推荐Markdown来写文章，我们最好下载[Markdownpad](http://markdownpad.com/download.html)来方便编写文章。
![markdownpad](/img/hexo_1_5.png)
左边是我们的markdown编写区，右边则是实时的效果展示区。
关于markdown的语法我以后有空再写篇文章介绍下。
编写你的文章后，还是老命令：

	hexo g 	//编译生成文件
	hexo s	//本地预览
	hexo d	//同步文件到Github
就这样，你已经能完成在你的博客上写文章让其他人来观看了。

## 其他
如果你看腻了Hexo的默认主题，可以去[官方的主题网页](https://hexo.io/themes/)中寻找你喜欢的主题，然后学习如何改进UI和样式效果，就能打造真正意义上的独立博客了。

当然，hexo并不止这些内容，记得多去官方文档看看，有很多特性等你去发掘。
