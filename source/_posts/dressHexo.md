---
title: 配置BLOG的一些小功能
categories: 其他
date: 2020-03-08 10:23:50
comments: false
toc: true
reward: false
tags: 
	- Hexo
	- NexT
---

## 0x01 前言

主要是搜集一些装扮BLOG界面的方法，将一些经过验证的配置方法放在这里，留着以后可以备用.

## 0x02 鼠标点击出现小红心的设置

在/themes/next/source/js/src下新建文件clicklove.js，接着把下面的代码拷贝到clicklove.js文件中。

``` bash
!function(e,t,a){function n(){c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),o(),r()}function r(){for(var e=0;e<d.length;e++)d[e].alpha<=0?(t.body.removeChild(d[e].el),d.splice(e,1)):(d[e].y--,d[e].scale+=.004,d[e].alpha-=.013,d[e].el.style.cssText="left:"+d[e].x+"px;top:"+d[e].y+"px;opacity:"+d[e].alpha+";transform:scale("+d[e].scale+","+d[e].scale+") rotate(45deg);background:"+d[e].color+";z-index:99999");requestAnimationFrame(r)}function o(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),i(e)}}function i(e){var a=t.createElement("div");a.className="heart",d.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:s()}),t.body.appendChild(a)}function c(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function s(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var d=[];e.requestAnimationFrame=function(){return e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)}}(),n()}(window,document);
```

在\themes\next\layout\\\_layout.swig文件末尾添加如下代码：

``` bash
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/clicklove.js"></script>
```

至此完成添加点击出现小红心的操作。

## 0x03 将某些文件置顶的方法

在node_modules/hexo-generator-index/lib/generator.js中将内容替换为如下代码：

``` bash
'use strict';

var pagination = require('hexo-pagination');

module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });

  var paginationDir = config.pagination_dir || 'page';
  var path = config.index_generator.path || '';

  return pagination(path, posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```

添加置顶标志，修改next\layout\\\_macro下的post.swig，找到<div class="post-meta"\> ，在下面添加对应代码：

```bash
<div class="post-meta">
          {% if post.top %}
            <i class="fa fa-thumb-tack"></i>
            <font color=808080>置顶</font>
            <span class="post-meta-divider">|</span>
          {% endif %}
     ......
```

在文章内容开头，添加 top: 100 表示置顶的优先级，级数越高越在顶层。

至此完成了添加文件置顶的功能。

## 0x04 对秘密的文章进行加密，以后可以安心的只给某人看啦

点击[这个链接](https://github.com/MikeCoder/hexo-blog-encrypt)，按步骤安装hexo-blog-encrypt插件。

``` bash
npm install --save hexo-blog-encrypt
```

安装完成后，在根目录中的package.json文件查看这个依赖是否存在。

``` bash
"hexo-blog-encrypt": "^3.0.12",
```

在配置文件(\_config.yml)中开启加密功能，若没有则添加如下代码：

``` bash
# Security
encrypt:
    enable: true
```

最后在文章中的头部信息中添加对应的信息即可。

``` bash
password: xxxx
```

至此完成了加密文章内容的小功能。
