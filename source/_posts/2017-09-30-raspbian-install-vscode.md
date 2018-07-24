---
title:  Raspbian安装vscode
date: 2017-09-30 10:13:50
tags: [树梅派, vscode] 
---

vscode是一个跨平台，轻量级，开源免费，具有强大插件支持编辑器，这不是微软的风格。经过长时间的使用，笔者现在已经爱上这款编辑器，之前一直用的是sublime text2, 现在基本舍弃之。

vscode应该是目前为止最适合开发angular的编辑器，安装上相关的插件后会有代码提示，代码检查，代码自动完成，自动模块导入，自动更正等。此外，我还喜欢使用vscode来写markdown文档，写shell scirpt, python, php等。安装相关的插件后，vscode简直是一款神器！

这篇文章暂时不会具体介绍vscode的用法，这里主要是介绍如何在树梅派上安装vscode。

<!-- more -->

在树梅派上安装vscode我是走了不少弯路， 由于官网没有编译好支持armhf的安装包供下载，只好自己去git clone下来源代码进行编译。
这里有篇文章介绍，不过可惜的是花了很长时间还是编译不出来，最后不得不放弃。  
[https://www.hanselman.com/blog/BuildingVisualStudioCodeOnARaspberryPi3.aspx](https://www.hanselman.com/blog/BuildingVisualStudioCodeOnARaspberryPi3.aspx)

没法自己编译，那就找找有没有好心人编译好安装包供下载的。果然在网上找到一位大牛提供的安装包，这位大牛还专门写了个网站介绍安装方法。
[https://code.headmelted.com/](https://code.headmelted.com/)

我是用的Rapbian, 使用的是下面命令进行安装

    . <( wget -O - https://code.headmelted.com/installers/apt.sh )

安装过程会遇到需要添加force命令参数的错误，可以先wget apt.sh把脚本下载到本地上修改一下再安装，提示permission问题的话加上sudo给予权限就可以了。

 安装成功后直接在终端输入 code-oss 启动vscode就可以了。启动时间还可以接受，响应速度还不错，安装了很多插件，暂时还没发现会对响应速度造成大的影响。非常推荐安装！

*The End*

