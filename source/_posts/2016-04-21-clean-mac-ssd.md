---
title: Macbook SSD硬盘空间不够用了？来个Xcode大瘦身吧！
date: 2016-04-21 22:24:38
tags: [Xcode, macbook, ssd]
---


## 写在前面
最近突然发现我的128G SSD硬盘只剩下可怜的8G多，剩下这么少的一点空间连Xcode都无法更新。怎么办呢？如果升级硬盘的话，第一要花钱，毕竟SSD硬盘还是不便宜，第二是升级比较麻烦，要拆机和迁移系统什么的特别花时间精力，老了真不愿瞎折腾了，只能想办法能不能清除点空间来。

<!-- more -->

## 寻找大块头
首先想到的就是能不能删掉安装在SSD硬盘里面平时不用或者很少用到软件，可是仔细一看发现每个软件都是精心挑选，辛辛苦苦安装上去的，看来看去都不舍得卸载掉；好吧，那就看看能不能清掉下载文件夹里面某些没用的文件，最后删来删去也就删了几百兆，后来才想起来好像之前硬盘空间紧张已经清理过一次了…

继续在硬盘里寻找可以清除的文件，不久大发现来了，用户文件夹下的 `资源库` 这个文件夹特别大，进到这个文件夹发现大块头是 `Developer` 文件夹，原来是Xcode耗费了我那么多空间！

## 开始清理
下面这几个是我主要清理的文件夹：

1. 这里放的是连接真机生成的文件，可以全部删掉或者把不常用的版本删掉，再次连接设备会自动生成

		~/Library/Developer/Xcode/iOS DeviceSupport

2. app打包生成的文件，可以删掉不需要的项目打包文件

		~/Library/Developer/Xcode/Archives

3. 项目的索引文件等，可以全部删除，或者删除不常用的项目，再次打开项目会自动生成

		~/Library/Developer/Xcode/DerivedData

经过清理，硬盘可用空间一下子变成了28G多，整整多了20多G！

## 最后
如果你也和我一样空间不够又不想升级硬盘，赶紧看看你的 `~/Library/Developer/Xcode/` 文件夹，如无意外应该也有惊喜。

The End.