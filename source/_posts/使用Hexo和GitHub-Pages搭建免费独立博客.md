---
title: 使用Hexo和GitHub Pages搭建免费独立博客及域名配置
date: 2016-11-11 16:31:52
categories:
	- hexo
tags:
	- hexo
	- github
	- blog
---


摘要：这是一篇使用GitHub Pages和Hexo搭建免费独立博客的总结。

我在这里写下长篇大论，只希望小白们能更快速入门。一天搭建出属于自己的个人独立博客，我将会通过 安装流程主线+优质文章 作为参考。从我个人接触到成功搭建博客，走了很多弯路，网上的资料更是琳琅满目无从下手，希望通过本教程给想搭建个人博客的人一个敢于尝试的机会。我会将这篇教程写仔细，会将我出现过的问题给予解决方法。大家有问题可以留言，我会尽量帮助大家解决。




# 前言
为什么选择GitHub Pages？
> * 无需购置服务器，目前的blog挂载在Github Pages，免服务器费的同时还能做负载均衡;
* github pages有300M免费空间，资料自己管理，保存可靠；
* 学着用github，享受github的便利，上面有很多大牛，眼界会开阔很多；
* 顺便看看github工作原理，最好的团队协作流程；
* github是趋势；


<!--more-->

# 准备工作
相信自己，敢于面对，过程并不是很难。

## Nodejs环境包

因为 Hexo 是基于 Node.js 的第三方模块，所以缺少 Node.js 不可。访问 [Node.js官网](http://nodejs.org/)下载适合自己系统的 Node.js 安装包。目前最新的版本为 ｖ6.5.0。

（注：至于安装过程和环境变量配置请参考[菜鸟教程-Node.js安装配置](http://www.runoob.com/nodejs/nodejs-install-setup.html)）

## Git工具包

如果之后你需要安装一些 Hexo 的主题和插件，[Git](https://git-scm.com/downloads/) 是最好的下载方式。因为好多主题都被放在了 Github 上，你只需要敲几个字符就可以下载。

（注：宁浩网之前介绍过Git的使用方法，安装过程及简单使用请见[这里](http://ninghao.net/blog/1379)）

### Git与GitHub区别

这里，我们要区分清楚git与github。
git是一个版本控制的工具，而github有点类似于远程仓库，用于存放用git管理的各种项目。

### 与GitHub建立联系

git安装好以后执行以下步骤：
> 1. 从程序目录打开 "Git Bash" ,或者直接用git shell，github自带的工具
2. 键入命令：ssh-keygen -t rsa -C "email@email.com"
  "email@email.com"是github注册账号邮箱地址
3. 提醒你输入key的名称，你可以不用输入，直接3个回车，就OK了；
4. 在C:\Documents and Settings\Administrator\下产生两个文件：id_rsa和id_rsa.pub
5. 用记事本打开id_rsa.pub文件，复制内容，在github的网站上找到ssh密钥管理页面，添加新公钥 。
6. 在git bash中输入ssh -T git@github.com命令，出现Hi username! You've successfully authenticated表示成功。

### 设置用户信息

现在你已经可以通过 SSH 链接到 GitHub 了，还有一些个人信息需要完善的。

Git 会根据用户的名字和邮箱来记录提交。GitHub 也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的，名字必须是你的真名，而不是GitHub的昵称。
```
$ git config --global user.name "***"//用户名
$ git config --global user.email "****@****.com"//填写自己的邮箱
```

# 开始搭建

因为最终博客是要部署到github上的，这里我首先讲解github建立仓库，然后讲解hexo安装。为了方便大家一次部署成功并且考虑到以后如果大家换电脑或者重装系统后还能够修改以前的博客，请按照我的解决方案进行，这里大家不懂也没事，照着来就行，我会在文章末尾优化部署与管理中详解。

## 创建GitHub Pages 仓库

在自己的GitHub账号下创建一个新的仓库，命名为username.github.io
（username是你的账号名)。在这里，要知道，GitHub Pages有两种类型：User/Organization Pages 和 Project Pages，而我所使用的是User Pages。
简单来说，User Pages 与 Project Pages的区别是：
> * User Pages 是用来展示用户的，而 Project Pages 是用来展示项目的。
* 用于存放 User Pages 的仓库必须使用username.github.io的命名规则，而 Project Pages 则没有特殊的要求。
* User Pages 将使用仓库的 master 分支，而 Project Pages 将使用 gh-pages 分支。
* User Pages 通过 http(s)://username.github.io 进行访问，而 Projects * Pages通过 http(s)://username.github.io/projectname 进行访问。

** 这一步很关键 **
创建两个分支：master 与 hexo。** 设置hexo为默认分支 **（因为我们只需要手动管理这个分支上的Hexo网站文件）

到这为止，我们的github仓库已经建立好了，我们马上就能见到成果了，下面我们开始建站。
## hexo介绍
Hexo 是一个轻量的静态博客框架。通过Hexo可以快速生成一个静态博客框架,仅需要几条命令就可以完成,相当方便。

而架设Hexo的环境更简单了 不需要 lnmp/lamp/XAMPP 这些繁琐复杂的环境 仅仅需要一个简单的http服务器即可使用 或者使用互联网上免费的页面托管服务

比如本人的这个博客 就是托管于 GitHub Pages服务上
## hexo安装
安装Hexo相当简单。在安装之前，必须检查电脑中是否已经安装下列应用程序：
* [Node.js](http://nodejs.org/)
* [Git](http://git-scm.com/)

如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。打开git bash执行以下命令：
```
$ npm install -g hexo-cli
```
这样hexo就已经安装好了。

## 使用hexo建站

安装完后，在你喜欢的文件夹内（我的是根目录）（例如H：\），点击鼠标右键选择Git bash，输入以下指令（填自己的地址）：
``` 
git clone git@github.com:username/username.github.io.git 
```

该命令会把你的博客仓库同步下来，然后cd到你的仓库文件夹下面依次执行以下命令：
> 1、$ hexo init

该命令会在目标文件夹内建立网站所需要的所有文件。接下来是安装依赖包：
> 2、$ npm install

这样，我们就已经搭建起本地的Hexo博客了。可以先执行以下命令（在对应文件夹下），然后再浏览器输入localhost:4000查看。
> 3、$ hexo generate
> 4、$ hexo server

这个博客只是本地的，别人是浏览不了的，之后需要部署到GitHub上。

### 相关资料

* [Hexo 官方文档](https://hexo.io/zh-cn/docs/)

## 部署博客到GitHub上

部署其实很简单，只要改一下配置文件，执行几条命令就行了，为了以后的方便，现在麻烦了一点，大家跟着做就行了，具体原因也在配置管理与优化里有讲到。

### 配置站点文件

我们继续使用上面的文件夹H:\username.github.io（也可以新建一个文件夹重新生成），然后编辑该文件夹下的_config.yml（这是站点配置文件）
默认生成的_config.yml：
``` 
# Deployment 
## Docs: http://hexo.io/docs/deployment.html 
deploy:   
  type:
```
修改后的_config.yml：（也是填入自己的ssh地址）
``` 
deploy:
  type: git
  repository: git@github.com:username/username.github.io.git
  branch: master
```
这里解释一下前面为什么建立两个分支master和hexo，为了管理方便，以后master分支用来发布网站（一会再说怎么发布），hexo分支用来存放Hexo网站文件。
### 发布
为了能够使Hexo部署到GitHub上，需要安装一个插件：（在项目目录下执行命令）
``` 
$ npm install hexo-deployer-git --save 
```
然后，执行下列指令即可完成部署：（以后发布也按照这三条命令执行）
``` 
$ hexo clean #清空public文件夹下生成的静态文件和db.json文件
$ hexo generate #重新生成静态文件和db.json
$ hexo deploy #按照站点配置文件部署到github上
```
之后，可以通过在浏览器键入：username.github.io进行浏览，开心吧~

### 提交Hexo网站文件到hexo分支

由于上面执行了hexo init命令，所以要重新关联远端库
首先在项目文件夹下执行以下命令：
``` 
$ git init #初始化为一个git目录
$ git remote add origin git@github.com:username/username.github.io.git #使用你自己的地址关联
$ git pull #pull一下你的远端库
```
此时你应该在hexo分支下，如下：
``` 
$ H:\username.github.io (hexo) (hexo-site@0.0.0)
```
如果不是，执行以下命令切换到hexo分支：
``` 
$ git checkout hexo 
```
然后执行以下命令提交网站相关文件：
```
$ git add . #添加所有文件到暂存区
$ git commit -m "提交信息"     #提交到本地仓库
$ git push origin hexo  #把本地库push到远端库的hexo分支
```
提交后去github上查看是否成功，这是我的[github地址](https://github.com/username/username.github.io)，看看是否一样。
## 更换主题

我使用的是[next主题]()，大家喜欢也可以去我的[github](https://github.com/username/username.github.io)上fork一下，然后在这基础上修改，大家也可以找自己喜欢的主题来换。
如果想要使用其他主题，可以使用git clone将别人的主题拷贝到H:\username.github.io\themes下，然后将_config.yml中的theme: landscape改为对应的主题名字。
下面以切换next主题为例来讲一下具体如何操作，同样也是在项目文件夹下执行以下命令：
```
$ git clone https://github.com/iissnan/hexo-theme-next.git themes/next
```
然后在站点配置文件_config.yml中的theme: landscape改为theme: next，重新发布一下就完成了。
### 相关资料
* [next主题使用文档](http://theme-next.iissnan.com/)


# 优化部署与管理
## 概述
Hexo部署到GitHub上的文件，是.md（你的博文）转化之后的.html（静态网页）。因此，当你重装电脑或者想在不同电脑上修改博客时，就不可能了（除非你自己写html）。
其实，Hexo生成的网站文件中有.gitignore文件，因此它的本意也是想我们将Hexo生成的网站文件存放到GitHub上进行管理的（而不是用U盘或者云备份啦。）这样，不仅解决了上述的问题，还可以通过git的版本控制追踪你的博文的修改过程，是极赞的。
但是，如果每一个GitHub Pages都需要创建一个额外的仓库来存放Hexo网站文件，我感觉很麻烦（10个项目需要20个仓库）。
所以，我利用了分支！！！
简单地说，每个想建立GitHub Pages的仓库，起码有两个分支，一个用来存放Hexo网站的文件，一个用来发布网站。
下面以我的博客作为例子详细地讲述。
### 我的博客搭建流程
> 1、创建仓库，username.github.io；
2、创建两个分支：master 与 hexo；
3、设置hexo为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；
4、使用git clone git@github.com:username/username.github.io.git拷贝仓库；
5、在本地username.github.io文件夹下通过Git bash依次执行npm install hexo-cli、hexo init、npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;
6、修改_config.yml中的deploy参数，分支应为master；
7、使用git init 、
git remote add origin git@github.com:username/username.github.io.git以及git pull命令重新关联远端库。
8、使用git checkout hexo命令切换到hexo分支然后依次执行git add .、git commit -m “…”、git push origin hexo提交网站相关的文件；
9、执行hexo generate -d生成网站并部署到GitHub上。


这样一来，在GitHub上的username.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。完美！
## 我的博客管理流程
### 日常修改
在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：
依次执行git add .、git commit -m “…”、git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）；
然后才执行hexo generate -d发布网站到master分支上。
虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催….的情况，调转顺序就有问题了）。
### 本地资料丢失
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：
> 1、使用git clone git@github.com:username/username.github.io.git拷贝仓库（默认分支为hexo）；
2、在本地新拷贝的username.github.io文件夹下通过Git bash依次执行下列指令：npm install hexo-cli、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）。

# 结尾
在网上看了很多资料，总结了很多资料，好累(-.-)