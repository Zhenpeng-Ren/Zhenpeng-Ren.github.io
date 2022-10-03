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


## 0x02 Tailscale

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


## 0x03 其他


