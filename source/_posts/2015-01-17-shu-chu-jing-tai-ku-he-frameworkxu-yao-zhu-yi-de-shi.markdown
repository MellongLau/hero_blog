---
layout: post
title: "输出静态库和framework需要注意的事"
date: 2015-01-17 18:04:11 +0800
comments: true
categories: iOS
---
### 问题
-----
最近因为项目需要，经常要打包静态库给别人用，同时静态库本身要加入其他同事做好的静态库，在build的时候发现比较多的问题就是提示:

    Symbol(s) not found for architecture arm64

<!--more-->

### 原因
-----
原因是打包静态库时直接用的时debug环境，而在该环境下默认设置`Build Active Architecture Only`为YES。

### 解决
-----
解决的方法有两种：

1.修改编译环境为release，再打包。   
2.修改`Build Active Architecture Only`中debug的值为NO。


到底选择哪种方法，具体还得看需求而定了。