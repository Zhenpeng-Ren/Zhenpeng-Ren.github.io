---
title: 博客搭建过程小记
categories: 其他
date: 2017-10-14 16:18
comments: true
toc: true
reward: true
tags:
	- Hexo
	- Github
---



## 0x01 环境准备 ##

本次博客的搭建主要借助于 Github 平台，利用 Hexo 搭建自己免费简易的博客。同时完成的是将博客备份在 Github 同一个仓库的不同分支上。

### 1.1 操作系统：Windows 10

### 1.2 Git版本：2.12.0.windows.1

在此可以下载 [Git](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git) 进行安装，主要的配置可以 [在此](https://segmentfault.com/a/1190000002645623) 查看。

### 1.3 Node.js的版本：v6.11.4

在此可以下载 [Node.js](https://nodejs.org/en/) 进行安装，安装完成即可。

### 1.4 npm的版本：3.10.10

npm 是随同 NodeJS 一起安装的包管理工具，能解决 NodeJS 代码部署上的很多问题。



## 0x02 本地搭建过程

我的过程是先在Github上面新建一个仓库，命名必须为【[your_user_name].github.io】,选中初始化的README选项，完成创建仓库。在Github中新建一个分支为hexo，并将分支hexo设置为默认分支（具体方法可找度娘）。此时就有两个分支：master分支用于存储生成的网页；hexo分支用于存储源文件的备份。

之后在电脑的文档文件夹内右键选择【Git Bash Here】，在弹出的 shell 内输入以下内容：

``` bash
$ git --version        #检查Git的版本
$ node -v              #检查NodeJS的版本
$ npm -v               #检查npm的版本
$ git clone git@github.com:[your_user_name]/[your_user_name].github.io.git
```

将克隆下来的文件夹中的隐藏文件【.git】和【README】文件剪切出来，放在一个临时的位置，等会儿再用。
之后在Git的shell中操作：

``` bash
$ cd Zhenpeng-Ren.github.io
$ npm install -g hexo-cli       #使用 npm 完成 Hexo 的安装
$ hexo init                     #在目标文件夹内建立网站所需要的所有文件
$ npm install                   #安装所需的依赖包
```

这样，我们就已经搭建起本地的 Hexo 博客了。可以先执行以下命令（在对应文件夹下），然后再浏览器输入localhost:4000查看效果。

``` bash
$ hexo generate
$ hexo server
```

到此，我们已经在本地安装了一个博客，但是别人无法浏览，之后会将博客部署在 Github 上面。



## 0x03 将博客部署在 Github 上面

在此之前，需要将之前移走的隐藏文件【.git】移到【[your_user_name].github.io】文件夹中，可以不要【README】文件。之后在当前文件夹内右键选择【Git Bash Here】，此时应该显示的是 hexo 分支。

在【[your_user_name].github.io】文件夹中，找到【_config.yml】文件，用notepad++打开修改以下几行为下面这种形式：

``` bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: 对应仓库的SSH地址（可以在GitHub对应的仓库中复制）
  branch: master    #分支（User Pages为master，Project Pages为gh-pages）
```

为了能够使Hexo部署到GitHub上，需要安装一个插件，在Git的shell中执行：

``` bash
$ npm install hexo-deployer-git --save
```

然后，执行下列指令即可完成部署：

``` bash
$ hexo generate
$ hexo deploy
```

之后，可以通过在浏览器键入：[your_user_name].github.io进行浏览。到此，就已经完成了博客的搭建，并将博客的源文件备份到了Github上面的hexo分支。



## 0x04 日常修改

在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：

1. 依次执行git add .、git commit -m “…”、git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）；
2. 然后才执行hexo generate -d发布网站到master分支上。


``` bash
#小注：每次更新博客也可以用下面的方法：
$ hexo clean
$ hexo generate
$ hexo deploy
```



## 0x05 在其他电脑上更新博客

使用下列步骤完成在其他电脑上博客的更新：

1. 使用git clone git@github.com:[your_user_name]/[your_user_name].github.io.git拷贝仓库（默认分支为hexo）；
2. 在本地新拷贝的【[your_user_name].github.io】文件夹下通过【Git bash】依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）



## 后记

本博客的搭建过程参考了 [CrazyMilk](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more) 的方法，在此给予感谢！