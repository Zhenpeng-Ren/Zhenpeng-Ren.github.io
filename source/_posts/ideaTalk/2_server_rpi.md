---
title: Server In RPi
categories: 学习琐事
date: 2021-10-30 22:00:30
comments: false
toc: true
reward: false
top: 
tags:
	- Raspberry
	- server
---

## 0x01 [来源之处](https://mp.weixin.qq.com/s/x0LC8D5InejKkGoIkC3bgQ)

### 配置放置blog的环境

## 0x02 安装nginx

```bash
#安装
sudo apt-get install nginx
#启动
sudo /etc/init.d/nginx start
#重启
sudo /etc/init.d/nginx restart
#停止
sudo /etc/init.d/nginx stop
```

将blog目录整体放到/var/www/目录下。
```bash
sudo mv blog/ /var/www/
```
```bash
sudo touch /etc/nginx/conf.d/blog_in_rpi.conf
```
```bash
sudo nano /etc/nginx/conf.d/blog_in_rpi.conf

server {
        listen 8080;
        root /var/www/blog_in_rpi;
}
```
```bash
sudo service nginx restart
```

浏览器打开：192.168.0.1:8080

[参考](https://blog.csdn.net/TyCoding/article/details/80480541)


## 0x03 内网穿透

内网穿透目前主要由ngrok和frp两种，国内ngrok免费的有ittun、sunny和natapp，前两种可以自定义域名。

### [ittun的ngrok_arm版本](http://ittun.com)

配置ittun:
```bash
./ngrok -proto=tcp 22
./ngrok -subdomain [demo] [8080]
./ngrok -config config.yml start alauda
```

### [sunny的arm版本](https://www.ngrok.cc)









## 没必要进行下一步了，下面为配置编写blog的环境。

## 0x04 安装[nodejs ARMv7](https://nodejs.org/en/download/)

### 下载node

```bash
wget https://nodejs.org/dist/v16.13.0/node-v16.13.0-linux-armv7l.tar.xz
```

### 解压nodejs

```bash
sudo mkdir -p /usr/local/lib/nodejs
sudo tar -xJvf node-v16.13.0-linux-armv7l.tar.xz -C /usr/local/lib/nodejs
```

### 设置环境变量

```bash
# Nodejs
VERSION=v16.13.0
DISTRO=linux-armv7l
export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
```

### 更新profile

```bash
. ~/.profile
```

### 测试node

```bash
node -v
npm version
npx -v
```

## 0x05 下载blog

```bash
git clone 
```
```bash
npm install hexo
npm install
npm install hexo-deployer-git
```

## 0x06 开始

```bash
$ hexo clean
$ hexo generate
$ hexo deploy
```
