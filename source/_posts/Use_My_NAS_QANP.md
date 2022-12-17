---
title: QANP NAS常用软件安装
categories: 学习琐事
date: 2022-05-23 23:23:08
comments: false
toc: true
reward: false
top: 
tags:
	- QANP
	- NAS
---

## 0x01 Transmission

教程链接: [在Container Station中安装Transmission](https://zhuanlan.zhihu.com/p/66422126)

Container Station中搜索Transmission，选择并安装linuxserver/transmission

高级配置中（前提是先在file station中创建共享文件夹，并新建三个目录）

进入左边共享文件夹栏，在挂载本机共享文件夹中新增三项：
==> /config      Transmission的配置文件目录。
==> /downloads   下载的文件将被放在这。
==> /seeds       存放新种子文件目录

其余的选择默认的即可，安装完毕后，点击链接即可进入下载页面。


## 0x02 Tailscale(Docker)

教程链接：[在Container Station中安装Tailscale](https://www.youtube.com/watch?v=OO0TcYGi0rc)

可以实现非局域网下访问NAS。

（1）Container Station中搜索Tailscale，选择并安装tailscale/tailscale

（2）在高级设置中NET选择Host模式后，确定新建

（3）点击Tailscale后，在控制面板处复制显示出来的链接，打开链接后[登录Tailscale](https://login.tailscale.com/login?next_url=%2Fwelcome)之后再确认

（4）手机中安装Tailscale的APP，复制NAS的ip地址

（5）Qfile中用这个ip地址登录，输入账号密码即可远程访问


## 0x03 Tailscale(应用软件)

下载最新的[Tailscale_v1.24.2_x86_64.qpkg](https://github.com/ivokub/tailscale-qpkg/releases/tag/v1.24.2)

QTS中APP Center，在设置中打开第三方软件安装许可(.qpkg)

教程链接: [Tailscale QPKG builder](https://github.com/ivokub/tailscale-qpkg)

首先打开QANP中的SSH功能，然后命令行中使用ssh连接NAS。

``` bash
$ ssh 鸢什之家@192.168.0.110

$ getcfg SHARE_DEF defVolMP -f /etc/config/def_share.info
(e.g. /share/CE_CACHEDEV1_DATA/)

$ cd /share/CE_CACHEDEV1_DATA/.qpkg/Tailscale

$ ./tailscale -socket var/run/tailscale/tailscaled.sock up
```

使用串口输出的链接，认证登录完成后，即可。

手机端，电脑端均可下载对应APP，进行远程连接。


## 0x04 其他


