---
title: iOS开发大神必备的Xcode插件
date: 2016-04-30 12:27:52
tags: [Xcode插件]
---

## 写在前面
工欲善其事，必先利其器，iOS开发中不仅要学会Xcode的基本操作，而且还得学会一些Xcode的使用技巧，如掌握常用的快捷键等，还有就是今天要说到的Xcode插件，下面我就为大家介绍几款开发中比较常用的Xcode插件（此处应有掌声）。

<!-- more -->

## 插件推荐

### 1. AMAppExportToIPA
* 简介：
AMAppExportToIPA 是一款可以让你在Xcode的project navigator界面中直接右键点击xxx.app -> Export IPA就可以生成对应的IPA文件的Xcode插件。

* 演示图片：

![AMAppExportToIPA](https://camo.githubusercontent.com/5c97f9833b75c4da7cc3c4e3d6f65e27b7cdecb0/68747470733a2f2f7261772e6769746875622e636f6d2f4d656c6c6f6e674c61752f414d4170704578706f7274546f4950412d58636f64652d506c7567696e2f6d61737465722f53637265656e73686f74732f73637265656e73686f742e676966)

* Github地址：[https://github.com/MellongLau/AMAppExportToIPA-Xcode-Plugin](https://github.com/MellongLau/AMAppExportToIPA-Xcode-Plugin)

### 2. HOStringSense
* 简介：
可以完美编辑正则表达式，多行文本，HTML等字符串，还提供字符串长度快速提示。

* 演示图片：

![HOStringSense](https://github.com/holtwick/HOStringSense-for-Xcode/raw/master/StringDemoAnimation.gif)

* Github地址：[https://github.com/holtwick/HOStringSense-for-Xcode](https://github.com/holtwick/HOStringSense-for-Xcode)

### 3. MCLog
* 简介：
MCLog 是一款可以让你轻松过滤Xcode控制台日志输出的Xcode插件。虽然目前已经可以搜索到控制台日志输出的文本，但是仍然还有大量你不感兴趣的日志。MCLog是对此问题的一个简单解决方案。使用简单的字符串来过滤控制台，并显示你真正想看到的日志。

* 演示图片：

![MCLog](https://camo.githubusercontent.com/ecbdebd01af366c9a7d3c3d0850a164f127030a9/68747470733a2f2f7261776769746875622e636f6d2f79756875612d6368656e2f4d434c6f672f6d61737465722f4d434c6f6753637265656e73686f742e676966)

* Github地址：[https://github.com/yuhua-chen/MCLog](https://github.com/yuhua-chen/MCLog)

### 4. AMMethod2Implement
* 简介：
可以自动的将.h或者.m .mm里边需要写入的方法自动填充进来。可以选择要导入的方法，然后按 `Ctrl+A` 或者 `Edit > AMMethod2Implement > Implement Method`.就会自动填充方法.也可以自行设置快捷键。
目前版本支持h文件声明方法自动生成实现，m或者mm文件已写好的方法生成方法声明到h文件， `extern NSString * const`， `@select(method:)` 和 `[self methodName]` 实现代码生成。

* 演示图片：

![AMMethod2Implement](https://camo.githubusercontent.com/a4204f4a68198d87e1b46059d38536ae374a7e5a/68747470733a2f2f7261772e6769746875622e636f6d2f4d656c6c6f6e674c61752f414d4d6574686f6432496d706c656d656e742f6d61737465722f53637265656e73686f74732f696d706c656d656e745f73656c6563746f722e676966)

* Github地址：[https://github.com/MellongLau/AMMethod2Implement](https://github.com/MellongLau/AMMethod2Implement)

### 5. Auto-Importer
* 简介：
可以搜索和自动导入头文件的一款Xcode插件。

* 演示图片：

![Auto-Importer](https://github.com/citrusbyte/Auto-Importer-for-Xcode/raw/master/demo.gif)

* Github地址：[https://github.com/citrusbyte/Auto-Importer-for-Xcode](https://github.com/citrusbyte/Auto-Importer-for-Xcode)

### 6. ColorSense
* 简介：
具有可以用颜色选择面板直接插入颜色代码和颜色代码显示颜色预览功能。

* Github地址：[https://github.com/omz/ColorSense-for-Xcode](https://github.com/omz/ColorSense-for-Xcode)

### 7. VVDocumenter
* 简介：
VVDocumenter是一款输入`///`就会自动生成javadoc风格注释的Xcode插件。

* 演示图片：

![VVDocumenter](https://camo.githubusercontent.com/ca5518c9872e15b8a95b9d8c5f44bc331977d710/68747470733a2f2f7261772e6769746875622e636f6d2f6f6e65766361742f5656446f63756d656e7465722d58636f64652f6d61737465722f53637265656e53686f742e676966)

* Github地址：[https://github.com/onevcat/VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode)

### 8. AMLocalizedStringBuilder
* 简介：
AMLocalizedStringBuilder 是可以帮助你将语言本地化文件Localizable.strings生成object-c的类AMLocalizedString的Xcode插件，这样可以直接使用R_String.am_<#你的本地化字符串key#>获取对应key的值，还可以随时点击Alt或Option按键查看当前字符串的值。

* 演示图片：

![AMLocalizedStringBuilder](https://camo.githubusercontent.com/75d5f8f86f8e9173d0e5d2e7a3515150f771ec76/68747470733a2f2f7261772e6769746875622e636f6d2f4d656c6c6f6e674c61752f414d4c6f63616c697a6564537472696e674275696c6465722d58636f64652d506c7567696e2f6d61737465722f53637265656e73686f74732f73637265656e73686f742e676966)

* Github地址：[https://github.com/MellongLau/AMLocalizedStringBuilder-Xcode-Plugin](https://github.com/MellongLau/AMLocalizedStringBuilder-Xcode-Plugin)

### 9. R.swift
* 简介：
类似AMLocalizedStringBuilder，不过是swift版本的，功能也更丰富，不仅支持Localized strings映射，还支持其他资源的映射，支持的列表如下：
	* Images
	* Custom fonts
	* Resource files
	* Colors
	* Localized strings
	* Storyboards
	* Segues
	* Nibs
	* Reusable cells

* 演示图片：

![R.swift](https://github.com/mac-cain13/R.swift/raw/master/Documentation/Images/DemoUseImage.gif)

* Github地址：[https://github.com/mac-cain13/R.swift](https://github.com/mac-cain13/R.swift)

### 10. CopyIssue
* 简介：
方便你搜索的任何错误或警告的问题，可以复制完整的问题描述，或者可以自动打开你的默认浏览器并通过Google（默认快捷⇧⌥G）或Stackoverflow（默认快捷⇧⌥S）搜索你选择的问题。

* 演示图片：

![CopyIssue](https://github.com/hanton/CopyIssue-Xcode-Plugin/raw/master/screenshots/ScreenShot.png?raw=true)

* Github地址：[https://github.com/hanton/CopyIssue-Xcode-Plugin](https://github.com/hanton/CopyIssue-Xcode-Plugin)

## 如何安装
安装方法目前有两种：

1. 从github下载源代码进行安装  
	* $ git clone git@github.com:插件地址
   * 打开插件项目运行，运行成功后程序会自动把插件文件拷贝到这个路径下： 
   `~/Library/Application Support/Developer/Shared/Xcode/Plug-ins`。
	* 重新启动Xcode使插件生效。

2. 通过Xcode插件管理器 `Alcatraz` 进行安装，安装完成后也要重新启动Xcode使插件生效。


## 最后

随着Xcode的发展和iOS开发的红火，现在Xcode插件越来越多了，插件越来越多当然是好事，毕竟选择就更多，功能也更丰富了，不过，安装太多插件容易造成Xcode运行不稳定，因此，安装插件还是要根据自身需求选择稳定性比较好的插件（此处应有掌声）。

*The End*

