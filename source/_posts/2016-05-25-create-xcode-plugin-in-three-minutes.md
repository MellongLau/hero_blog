---
title: 三分钟编写一款Xcode插件
date: 2016-05-25 21:04:47
tags: xcode
---

## 写在前面
从事iOS开发也比较长时间了，起初的时候用了一些Xcode插件之后感觉确实对开发帮助挺大，后来开始对Xcode插件开发感兴趣了，于是先后制作了[AMMethod2Implement](https://github.com/MellongLau/AMMethod2Implement), [AMAppExportToIPA](https://github.com/MellongLau/AMAppExportToIPA-Xcode-Plugin) 和 [AMLocalizedStringBuilder](https://github.com/MellongLau/AMLocalizedStringBuilder-Xcode-Plugin) 这三款Xcode插件，这些都是在长期使用Xcode开发中萌发出的想法，后来经过研究开发出来的。现在很开心看到越来越多的人开始在开发Xcode插件，很多很有想法的插件开发出来了。同时我相信还有很多人对Xcode插件开发很感兴趣，但是却无从下手，于是有了这一篇文章。

<!-- more -->

## 如何开发
插件开发用到各种各样的技术，不是一篇文章可以说得完全的，这篇文章只能算是一个引子，所用到的是最简答的技术进行开发Xcode插件，而这种方法适合的也只是某种特定的场景：在Xcode中选中代码后可以对这些代码进行处理。


## 开始
### 效果预览

首先，我们来看一下完成的效果，在Xcode中选择一段要注释的代码，然后点击右键 `Services` -> `Comment Selected Text`，我们的插件自动将这段代码用 `/*   */` 注释掉，如下面演示图片所示。

![screenshot.gif](https://raw.githubusercontent.com/MellongLau/workflow-xcode-plugin/master/screenshot.gif)

### 动手制作
1. 在 `应用程序` 中打开系统自带的 `Automator` 应用，在`选取文稿类型`中选择服务后点击`选取`按钮。
2. 左侧的 `资源库` 中选中实用工具，并在右侧列表拉到底部双击选择 `运行shell脚本`。
3. 按下图所示进行修改：

![Thumbnail](https://raw.githubusercontent.com/MellongLau/workflow-xcode-plugin/master/Comment%20Selected%20Text.workflow/Contents/QuickLook/Thumbnail.png)

完成以上步骤后点击保存名为`Comment Selected Text`，至此，插件已经制作完成，现在打开Xcode的项目，在代码编辑界面选中一段代码，然后点击右键选择`Services`->`Comment Selected Text`，选中的代码自己被注释掉。

![选择文中后点击右键](/blogImages/xcode-plugin-01.png)

我已经把这个workflow文件保存到github上，你可以到这个地址下载：[https://github.com/MellongLau/workflow-xcode-plugin](https://github.com/MellongLau/workflow-xcode-plugin)

## 最后
今天介绍的是最简单实用的Xcode插件开发的方法，值得一提的是这个方法在其他的文本编辑器中也可以用，所以这个方法具有普遍的实用性。

如果这篇文章对你有帮助，请分享给更多人知道，转载请注明出处。

*The End*
