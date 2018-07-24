---
title: iOS为你的数字添加动画：AMAnimatedNumber控件
date: 2016-04-10 22:49:01
tags: [iOS, object-c]
---

## 效果预览

![screenshot.gif](https://raw.github.com/MellongLau/AMAnimatedNumber/master/Screenshots/screenshot.gif)

<!--more-->

## 安装

### 从CocoaPods安装

[CocoaPods](http://www.cocoapods.org) 推荐用CocoaPods来添加管理AMAnimatedNumber控件到你的项目中.

1. 添加到 Podfile 文件中.

  ```ruby
  pod "AMAnimatedNumber", "~> 0.0.1"
  ```

2. 使用pod命令 `pod install` 进行安装.
3. 导入 AMAnimatedNumber： `#import <AMAnimatedNumber.h>`.

## 使用方法

- 初始化:

```objc
AMAnimatedNumber *animateNumber = [[AMAnimatedNumber alloc] initWithFrame:CGRectMake(0, 0, 300, 35)];
[self.view addSubview:animateNumber];
```

- 更改数字和样式

```objc
[animateNumber setTextFont:[UIFont boldSystemFontOfSize:28]];
[animateNumber setTextColor:[UIColor brownColor]];

[animateNumber setNumbers:@"$ 89,572,234.32"
                  animated:YES];


```

控件的源码地址：
https://github.com/MellongLau/AMAnimatedNumber