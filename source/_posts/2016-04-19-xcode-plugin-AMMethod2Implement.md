---
title: Xcode自动填充方法插件：AMMethod2Implement
date: 2016-04-19 23:18:37
tags: [Xcode]
---

![AMMethod2Implement Banner](https://raw.github.com/MellongLau/AMMethod2Implement/master/Screenshots/banner.png)

## 简介

AMMethod2Implement是一款可以自动的将.h或者.m .mm里边需要写入的方法自动填充进来的Xcode插件。使用过程中可以选择要导入的方法，然后按 `Ctrl+A` 或者点击Xcode顶部的菜单 `Edit` > `AMMethod2Implement` > `Implement Method` 就会自动生成填充好选中的方法，快捷键默认是`Ctrl+A`，也可以自行设置快捷键。

目前版本支持h文件声明方法自动生成实现，m或者mm文件已写好的方法生成方法声明到h文件， extern NSString * const， @select(method:) 和 [self methodName] 实现代码生成，暂时只支持object-c语言，不支持swift。

<!-- more -->

## 使用演示
![usageScreenshot.gif](https://raw.github.com/MellongLau/AMMethod2Implement/master/Screenshots/usageScreenshot.gif)

![implement_selector.gif](https://raw.github.com/MellongLau/AMMethod2Implement/master/Screenshots/implement_selector.gif)

## 安装方法
安装方法目前有两种：

1. 从github下载源代码进行安装  
	* $ git clone git@github.com:MellongLau/AMMethod2Implement.git
   * 打开AMMethod2Implement项目运行，运行成功后程序会自动把插件文件拷贝到这个路径下： 
   `~/Library/Application Support/Developer/Shared/Xcode/Plug-ins`。
	* 重新启动Xcode使插件生效。

2. 通过Xcode插件管理器 `Alcatraz` 进行安装，安装完成后也要重新启动Xcode使插件生效。

## 安装后无法使用怎么办
如果安装后出现无法使用的情况，可以尝试在终端运行一下命令：
> curl https://raw.githubusercontent.com/cielpy/RPAXU/master/refreshPluginsAfterXcodeUpgrading.sh | sh

运行完重启Xcode再试。

插件地址：  
[https://github.com/MellongLau/AMMethod2Implement](https://github.com/MellongLau/AMMethod2Implement)
