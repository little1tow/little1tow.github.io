---
title: hexo+github搭建个人博客记录
date: 2018-05-26 21:28:13
tags: notes
---

作为一个工科生，但一直希望能够记录一些东西；同样作为一个工科生，有能力自己搭建博客。看了很多大神的github博客，最终决定也用hexo+github搭建一个自己的个人小站，记录下自己的一些足迹。

# 搭建步骤

## github建立repository

在自己的github上新建repository，要注意的是name必须是username.github.io，该地址也是博客的地址，换成其他名字是不行的

## hexo安装

安装git和node.js

> git地址：https://git-scm.com/download/win
>
> node.js地址：https://nodejs.org/zh-cn/

安装Hexo

> npm install -g hexo #-g表示全局安装, npm默认为当前项目安装

## Hexo部署

建立博客的根目录，（建议使用全英文目录，避免出现奇奇怪怪的bug），然后在该目录下打开git bash，（这里同样建议使用git bash，不使用windows terminal，因为windows terminal有一些log信息不会输出，这样就看不到哪里有问题了）

> hexo init #新建博客目录
> hexo g #根据当前目录下文件生成静态网页
> hexo s #启动服务器，还可以使用hexo s -p 4321，解决端口占用问题

这样就可以在浏览器中输入localhost:4000查看了，

简单介绍一下文件目录，摘自[这里](http://www.shuang0420.com/2016/05/12/Github-Pages-Hexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)

- public：执行hexo generate命令，输出的静态网页内容目录
- scaffolds：layout模板文件目录，其中的md文件可以添加编辑
- scripts：扩展脚本目录，这里可以自定义一些javascript脚本
- source：文章源码目录，该目录下的markdown和html文件均会被hexo处理。该页面对应repo的根目录，404文件、favicon.ico文件，CNAME文件等都应该放这里，该目录下可新建页面目录。
- drafts：草稿文章
- posts：发布文章themes：主题文件目录
- config.yml：全局配置文件，大多数的设置都在这里
- package.json：应用程序数据，指明hexo的版本等信息，类似于一般软件中的 关于 按钮

## Hexo 写文章

>  hexo new "postname" #然后在posts目录下的postname.md文件中编辑博客

然后在source目录下打开对应的markdown文件，编辑即可，这里建议使用[typora](http://www.typora.io)

## Hexo本地调试

> hexo clean #清除之前生成的内容，保证不出问题
> hexo g #生成
> hexo s #启动本地服务，进行文章预览调试，也可以使用hexo s -p 4321

## Hexo部署到github上

首先安装一个插件，保证能够使Hexo部署到GitHub上：

> npm install hexo-deployer-git --save

在博客目录下找到配置文件_config.yml，进行编辑

编辑前：
> \# Deployment
> \##  Docs: http://hexo.io/docs/deployment.html
> deploy:
>   type:

编辑后：
> deploy:
>   type: git
>   repo: 对应仓库的SSH地址（可以在GitHub对应的仓库中复制）
>   branch: 分支（User Pages为master，Project Pages为gh-pages）


然后执行：

> hexo g
> hexo deploy

之后就可以在浏览器中通过username.github.io进行浏览，棒棒哒

## 优化部署和管理

虽然目前已经基本搭建好了博客，但是编辑只能在当前电脑编辑，github上保存的是生成之后的html文件，整个博客的源代码都在本地，那如果想要在别的电脑上编辑怎么办，这时候就要利用到github的分支，即在建立博客仓库的时候，建立两个分支，一个用于展示网站内容，一个用于存放hexo文件，

具体流程参考了[这里](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)

### 创建流程

1. 创建仓库，username.github.io；
2. 创建两个分支：master 与 hexo；
3. 设置hexo为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；
4. 使用git clone git@github.com:username/username.github.io.git拷贝仓库；
5. 在本地username.github.io文件夹下通过Git bash依次执行npm install hexo、hexo init、npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;
6. 修改_config.yml中的deploy参数，分支应为master；
7. 依次执行git add .、git commit -m “…”、git push origin hexo提交网站相关的文件；
8. 执行hexo generate -d生成网站并部署到GitHub上。

这样一来，在GitHub上的CrazyMilk.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。

### 日常修改

在本地对博客进行修改，一般是如下步骤

1. git pull  (在保证本地没有修改的情况下，更新到github上的版本，保持版本一致，非常重要)
2. 进行各种编辑，修改操作
3. 依次执行git add .、git commit -m “…”、git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）
4. 然后才执行hexo generate -d发布网站到master分支上

### 异地修改

当在不同的电脑上修改时，一般是如下步骤

1. 使用git clone git@github.com:username/username.github.io.git拷贝仓库（默认分支为hexo）
2. 在本地新拷贝的username.github.io文件夹下通过Git bash依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git
3. 然后就是日常修改中的2以后的操作

# 最后

果然自己搭，坑不是一般的多，后续会慢慢更新，记录我在使用过程中踩过的坑↖(^ω^)↗