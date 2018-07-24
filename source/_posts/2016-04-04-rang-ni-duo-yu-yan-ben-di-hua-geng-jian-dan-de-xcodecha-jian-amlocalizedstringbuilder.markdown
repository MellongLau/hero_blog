---
layout: post
title: "让多语言本地化变得更简单的Xcode插件——AMLocalizedStringBuilder"
date: 2016-04-04 23:11:28 +0800
comments: true
categories: [Xcode插件]
tags: [Xcode]
---

## 写在前面

一直就想写一款多语言本地化的插件，虽然网上也有很多这种类型的插件可供选择，但是总感觉用起来不够方便。

一次偶然的机会接触到Android开发，觉得Android开发中直接可以使用R.string直接获取到定义在xml里面的文字资源，感觉很方便，于是就有个想法，Xcode也能否做到这样，最后经过研究开发出了这款插件。

<!--more-->

## AMLocalizedStringBuilder是什么
AMLocalizedStringBuilder 是可以帮助你将语言本地化文件Localizable.strings生成object-c的类AMLocalizedString的Xcode插件，这样可以直接使用R_String.am_<#你的本地化字符串key#>获取对应key的值，还可以随时点击Alt或Option按键查看当前字符串的值。

## 安装方法
安装方法目前有两种：

1. 从github下载源代码进行安装  
	* $ git clone git@github.com:MellongLau/AMLocalizedStringBuilder-Xcode-Plugin.git
   * 打开AMLocalizedStringBuilder项目运行，运行成功后程序会自动把插件文件拷贝到这个路径下： 
   `~/Library/Application Support/Developer/Shared/Xcode/Plug-ins`。
	* 重新启动Xcode使插件生效。

2. 通过Xcode插件管理器 `Alcatraz` 进行安装，安装完成后也要重新启动Xcode使插件生效。


## 安装后无法使用怎么办
如果安装后出现无法使用的情况，可以尝试在终端运行一下命令：
> curl https://raw.githubusercontent.com/cielpy/RPAXU/master/refreshPluginsAfterXcodeUpgrading.sh | sh

运行完重启Xcode再试。

## 如何使用
1. 点击Xcode顶部菜单 Product->AMLocalizedStringBuilder->Build Localized String 或者直接用快捷键 ctrl+f 来把Localizable.strings生成为object-c类。
2. 打开你当前项目文件夹，可以找到已经生成好的 AMLocalizedString.h and AMLocalizedString.m 这两个文件，把他们直接拉到项目中添加引用。
3. 在要用到的地方先导入头文件 AMLocalizedString.h， 然后使用R_String.am_<#your_localized_string_key#>来获取对应本地化的文字。
4. 另外，你还可以用快捷键ctrl+cmd+s打开设置窗口，在设置窗口里面选择你要进行转换的Localizable.strings文件。

可以参考以下步骤演示：

![screenshot.gif](https://raw.github.com/MellongLau/AMLocalizedStringBuilder-Xcode-Plugin/master/Screenshots/screenshot.gif)


插件地址：  
[https://github.com/MellongLau/AMLocalizedStringBuilder-Xcode-Plugin](https://github.com/MellongLau/AMLocalizedStringBuilder-Xcode-Plugin)