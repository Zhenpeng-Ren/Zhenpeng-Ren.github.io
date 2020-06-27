---
title: 树莓派的折腾笔记
categories: 其他
date: 2020-06-26 14:46:30
comments: false
toc: true
reward: false
top: 
tags:
	- Raspberry
	- Ubuntu 20.04
---

## 0x01 树莓派连接Wi-Fi

sudo ifconfig wlan0 up

sudo apt install wpasupplicant

sudo killall wpa_supplicant

sudo wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf

sudo apt install dhcpcd5

sudo dhcpcd wlan0


## 0x02 Ubuntu系统中添加新用户和Samba

sudo adduser renzhenpeng

sudo apt-get install samba

sudo apt-get install smbclient

sudo apt-get install smbfs

sudo vim /etc/samba/smb.conf 

sudo /etc/init.d/samba-ad-dc restart

sudo service smbd reload

sudo service smbd restart

sudo service nmbd restart










