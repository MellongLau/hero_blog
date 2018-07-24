---
title: 神器Docker入门之安装配置篇
date: 2016-04-28 21:13:18
tags: [docker]
---


![Docker](/blogImages/Logo-Docker.jpg)

## 写在前面

Docker近几年来火得不得了，作为一名IT人如果不知道Docker是什么就有点out了，确实，我也out了，这几天才知道的Docker。连忙网上一顿学习，才知道Docker是什么(⊙﹏⊙)b，如果你也不知道什么是Docker，也想试一试，那么这篇文章非常值得你一看。

<!-- more -->

## Docker是什么？

### Docker是何方神圣，为何如此之受欢迎呢？

> 拿现实世界中货物的运输作类比, 为了解决各种型号规格尺寸的货物在各种运输工具上进行运输的问题, 我们发明了集装箱。

Docker每个镜像相当于一个集装箱，当我们把配置好的环境交给客户时，我们只需要把镜像发给客户，客户不用再做环境配置的工作，也不用担心使用起来会和我们这边的环境不一样。其实这个也和虚拟机类似，不过虚拟机运行起来占用资源厉害，启动速度慢，镜像体积也比较大。

看到这里还是不明白，不用着急，继续往下看。

### Docker可以做什么？

Docker可以用来做演示，可以做环境备份，也可以把部署环境发给客户，对于我来说，最有用的就是利用Docker可以快速运行不同的软件进行学习和尝试。例如我想试试最新版本的WordPress，但是我又不想花一大堆时间去配置数据库，搭建php运行环境等等，这时用Docker的话，只要几条命令，花上几分钟（主要是下载镜像比较花时间）就可以用上WordPress，回想起以前第一次玩WordPress配置环境的时候是多么痛苦...

## 安装配置Docker

Mac OS X安装Docker非常简单，直接到Docker官网下载他家的 [Docker Toolbox](https://www.docker.com/products/docker-toolbox) ，根据安装提示安装即可，这里也有windows版的，不过遗憾的是目前Mac版和windows版都是基于虚拟机实现的，不用虚拟机的话只能Linux才可以做到。

安装完毕后点击 `Launchpad`，在里面打开`Docker Quickstart Terminal` 即可启动Docker，使用`Kitematic`也可以打开Docker，这是Docker的图形化管理界面，用起来也挺方便的。

## 运行第一个容器

### Hello World
安装配置好之后，我们已经迫不及待要运行一下容器来试试了，对于程序员来说，当然第一个程序应该是hello world了，官方已经为我们做了一个hello world的镜像，只需要一句命令就可以跑起来：

> $ docker run hello-world

输出信息如下：

```shell
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
535020c3e8ad: Pull complete
af340544ed62: Pull complete
Digest: sha256:a68868bfe696c00866942e8f5ca39e3e31b79c1e50feaee4ce5e28df2f051d5c
Status: Downloaded newer image for hello-world:latest

Hello from Docker.
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
1. The Docker Engine CLI client contacted the Docker Engine daemon.
2. The Docker Engine daemon pulled the "hello-world" image from the Docker Hub.
3. The Docker Engine daemon created a new container from that image which runs the
   executable that produces the output you are currently reading.
4. The Docker Engine daemon streamed that output to the Docker Engine CLI client, which sent it
   to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
$ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
https://hub.docker.com

For more examples and ideas, visit:
https://docs.docker.com/userguide/
```

看到这些信息也证明你已经成功安装了Docker。

### Ubuntu
眼尖的应该发现输出信息里面有一句

> $ docker run -it ubuntu bash

没错，这句命令就是运行一个Ubuntu容器，只要这一句命令Docker就自动会从Docker hub上下载最新的Ubuntu镜像到本地并且运行。然而，由于Docker的服务器在大洋彼岸，下载起来确实是比较慢，后面的文字会分享如何使用国内的镜像服务器进行下载。


## 最后

这篇文章主要是介绍Docker的安装配置，后面的文章会详细介绍如何使用国内镜像，运行WordPress、GitLab和Ghost等软件，如对Docker有兴趣请关注后续内容。

*作者也是第一次玩Docker，文中难免有错误之处，望各位多予指正，不胜感激。*


## 参考

1. [http://blog.csdn.net/colorant/article/details/20608157](http://blog.csdn.net/colorant/article/details/20608157)
2. [https://docs.docker.com/mac/step_one/](https://docs.docker.com/mac/step_one/)

*The End.*

