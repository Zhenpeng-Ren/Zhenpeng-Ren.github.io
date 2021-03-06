---
title: 惠普笔记本装双系统的小坑
categories: 其他
date: 2017-11-01 09:46:08
comments: false
toc: true
reward: false
top: 
tags:
	- Windows 10
	- Ubuntu 14.04
---

## 0x01 背景介绍

由于不想将惠普电脑装入盗版系统，惊奇的发现惠普电脑自带一个 *系统恢复* 的功能。

双系统采用的方式为：Windows 10 + Ubuntu 14.04



## 0x02 将惠普笔记本执行系统恢复过程

首先，需要将原有系统的资料做好备份，以免丢失。

[使用 HP Recovery Manager进行恢复](https://support.hp.com/cn-zh/document/c04777644)：

如果 Windows 正常打开，请执行以下步骤，通过 Windows“搜索”启动系统恢复。
1. 关闭电脑。
2. 断开所有连接设备和电缆，如个人便携式硬盘、U 盘、打印机和传真机。从内部驱动器中取出介质，并卸下所有最近添加的内部硬件（台式机的话，不要断开显示器、键盘、鼠标和电源线的连接）。
3. 启动电脑。
4. 从“搜索”界面，输入“Recovery Manager”，然后从搜索结果中选择“HP Recovery Manager”。
5. 在“帮助”下，请点击 Windows 恢复环境。
6. 点击“确定”。系统将重启进入 Windows 恢复环境。
7. 点击“故障排除”。
8. 点击“Recovery Manager”。
9. 选择“在没有备份文件的情况下进行恢复”，然后点击“下一步”。
10. 阅读系统恢复信息。要继续操作，请点击 “下一步”。
11. 完成准备工作。点击“继续”按钮。

至此，电脑将自动重启。按照屏幕说明操作，请勿在此期间中断操作。在恢复期间，电脑会重启多次，之后一个崭新的windows 10系统就展现出来了。



## 0x03 装入Ubuntu 14.04系统

### 3.1 准备的软件及工具

1. 下载 Ubuntu 14.04 [官方镜像](http://cdimage.ubuntu.com/netboot/14.04/?_ga=2.63084719.165761440.1509501681-1262211413.1509501681)
2. [rufus](https://rufus.akeo.ie/?locale) \(用于制作U盘镜像\)
3. 大于4G的U盘

### 3.2 磁盘空间初步分配

按下“WINDOWS”+“X”组合键，进入磁盘管理，选择一个空间大点的磁盘，右键选择“压缩卷”，给 Ubuntu 分配了50G的容量。

### 3.3 禁用快速启动

==》 左下角右键，菜单内找到电源选项
==》 点击“选择电源按钮的功能”
==》 点击“更改当前不可用的设置”
==》 取消“启用快速启动”

【注】：  “快速启动”是 Windows 10 为了加速自己的启动过程而跳过一些bios的检查。

### 3.4 禁用安全启动（Secure Boot）

==》 所有设置中选择“更新和安全”
==》 选择“恢复”
==》 选择“立即重启”
==》 选择“疑难解答”
==》 选择“高级选项”
==》 选择“启动设置”
==》 选择相应的选项关闭安全模式即可

也可以直接进入BIOS，在 Secure Boot 中将值更改为 Disabled 即可。

### 3.5 制作Ubuntu的启动U盘

插入U盘，打开准备好的 rufus ，选择Ubuntu 14.04的镜像，点击开始进行写入，稍等片刻等待写入完成。

### 3.6 U盘安装Ubuntu 14.04

惠普电脑是在开机时连续按 ESC 键，找到 USB 启动，电脑将进行重启。

其中的设置不用细说，下面着重讲述一下“安装类型”这一步：

1. 选择“其他选项”
2. 为空闲磁盘分区：在这一步会看到我们之前分配的未使用磁盘空间，我们即将为这块空闲磁盘分区。
	--- /：存储系统文件，建议10GB ~ 15GB，一般16GB即可；
	--- swap：交换分区，即Linux系统的虚拟内存，建议是物理内存的2倍；
	--- /home：home目录，存放音乐、图片及下载等文件的空间，建议最后分配所有剩下的空间；
	--- /boot：包含系统内核和系统启动所需的文件，实现双系统的关键所在，建议200M
	【注】：实际上，一块硬盘最多容纳4个主分区，或3个主分区外加1个扩展分区，在选择分区类型时，可能会出现“安装系统时空闲分区不可用”状况，为了解决问题，下面一律选择“逻辑分区”。
3. 最重要的一步：选择/boot对应的盘符作为“安装启动引导器的设备”，务必保证一致。

之后按照正常的步骤完成安装即可。



## 0x04 如何进入Ubuntu系统

安装完重启，发现还是进入了 win10 系统，无法进入 Ubuntu 系统，这时有一个方法可以进入 Ubuntu 系统，启动的时候按 ESC+F9 进入启动选择界面，可以看到有 win10 和 Ubuntu 的启动项，选择 Ubuntu 就可以启动了。

虽然可以使用双系统，但还是不方便，启动的时候按ESC稍慢一点就进入了win10。基于此，研究了一下EFI文件系统，EFI是一个单独的分区，在第一块磁盘的开始256M空间上，ubuntu启动后EFI文件系统mount到/boot/efi目录。

惠普笔记本在 BIOS 驱动中指定了从EFI/Microsoft/Boot/bootmgfw.efi启动，即使EFI下还有其他的系统启动项也不会从其他系统启动，因此可以修改此处达到我们的目的。

步骤如下：

1. 首先备份win10的启动项（在目录/usr/下）
2. 在同一目录下，复制WINDOWS文件夹并更名为win10
3. 将/中的grubx64.efi替换掉/中的bootmgfw.efi，这样替换之后grub就接管了系统的启动。
4. 打开一个终端，执行update-grub2来更新启动项，更新完毕以后在/boot/grub/grub.cfg文件中找到相应的位置，把windows改成win10就可以了。



## 0x05 后记

参考链接：
【1】[惠普客户支持](https://support.hp.com/cn-zh/document/c04777644)
【2】[折腾uefi下win10与Ubuntu 16.04 LTS双系统，完美兼容grub2引导](https://tinyslik.github.io/2017/02/10/win10+kylin%E5%8F%8C%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/)
【3】[暗影精灵2pro安装win10+ubuntu16.10双系统](http://blog.csdn.net/zyix_0712/article/details/69675748)
