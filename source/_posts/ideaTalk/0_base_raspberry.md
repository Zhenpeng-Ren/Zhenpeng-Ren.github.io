---
title: 树莓派的折腾笔记
categories: 学习琐事
date: 2021-10-24 22:11:30
comments: false
toc: true
reward: false
top: 
tags:
	- Raspberry
	- Wi-Fi/SSH
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

(2020-06-26 14:46:30)

## 0x03 配置RPi开机自动连网并开启ssh

配置原因：身边没有找到外接屏幕和键盘的情况

操作步骤：

（1）下载树莓派的操作系统[OS Lite](https://www.raspberrypi.com/software/operating-systems/)
（2）使用[balenaEtcher-1.5.122](https://www.balena.io/etcher/)烧写OS Lite到TF卡上
（3）使用读卡器将TF卡连到电脑上
（4）在根目录/boot下新建wpa_supplicant.conf，内容见附录A
（5）在根目录/boot下新建ssh文件，内容为空
（6）将TF卡插进树莓派进行系统启动，过一阵儿时间就可以在路由器的web页面查看到给树莓派分配的ip
（7）此时使用ssh pi@192.168.xx.xx，回车后输入默认密码raspberry，即可登录


### 附录A

``` bash
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
 
network={
	ssid="WiFi-A"
	psk="12345678"
	key_mgmt=WPA-PSK
	priority=1       # 优先级：数字越小，优先级越低
}
 
network={
	ssid="WiFi-B"
	psk="12345678"
	key_mgmt=WPA-PSK
	priority=2
	scan_ssid=1       # wifi为隐藏状态时，配置为1
}
```

## 0x04 python脚本获取树莓派的使用信息

```bash
import os

# Return CPU temperature as a character string

def getCPUtemperature():
	res = os.popen('vcgencmd measure_temp').readline()
	return(res.replace("temp=","").replace("'C\n",""))

# Return RAM information (unit=kb) in a list

# Index 0: total RAM
# Index 1: used RAM
# Index 2: free RAM

def getRAMinfo():
	p = os.popen('free')
	i = 0
	while 1:
		i = i + 1
		line = p.readline()
		if i==2:
			return(line.split()[1:4])

# Return % of CPU used by user as a character string

def getCPUuse():
	return(str(os.popen("top -n1 | awk '/Cpu\(s\):/ {print $2}'").readline().strip()))

# Return information about disk space as a list (unit included)

# Index 0: total disk space
# Index 1: used disk space
# Index 2: remaining disk space
# Index 3: percentage of disk used

def getDiskSpace():
	p = os.popen("df -h /")
	i = 0
	while 1:
		i = i +1
		line = p.readline()
		if i==2:
			return(line.split()[1:5])

# CPU informatiom
CPU_temp = getCPUtemperature()
CPU_usage = getCPUuse()


# RAM information
# Output is in kb, here I convert it in Mb for readability

RAM_stats = getRAMinfo()
RAM_total = round(int(RAM_stats[0]) / 1000,1)
RAM_used = round(int(RAM_stats[1]) / 1000,1)
RAM_free = round(int(RAM_stats[2]) / 1000,1)

# Disk information

DISK_stats = getDiskSpace()
DISK_total = DISK_stats[0]
DISK_used = DISK_stats[1]
DISK_perc = DISK_stats[3]

if __name__ == '__main__':
	print('')
	print('CPU Temperature = '+CPU_temp)
	print('CPU Use = '+CPU_usage)
	print('')
	print('RAM Total = '+str(RAM_total)+' MB')
	print('RAM Used = '+str(RAM_used)+' MB')
	print('RAM Free = '+str(RAM_free)+' MB')
	print('')
	print('DISK Total Space = '+str(DISK_total)+'B')
	print('DISK Used Space = '+str(DISK_used)+'B')
	print('DISK Used Percentage = '+str(DISK_perc))

```

(2021-10-24 22:11:30)


