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

## 0x03 安装[nodejs ARMv7](https://nodejs.org/en/download/)

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

## 0x04 下载blog

```bash
git clone 
```
```bash
npm install hexo
npm install
npm install hexo-deployer-git
```

## 0x05 开始

```bash
$ hexo clean
$ hexo generate
$ hexo deploy
```
