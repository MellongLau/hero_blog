---
title: 修复ios8 gps关闭无法跳转到系统设置页面问题
date: 2016-04-19 20:58:42
tags: [iOS]
---


之前一直使用
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"prefs://"]];
方便用户点击按钮后跳转到系统设置页面，最近发现有的机器无法跳转成功。

原来是ios8打后用了不同的url string进行跳转，修复兼容代码如下：

```objc
double version = [[UIDevice currentDevice].systemVersion doubleValue];//判定系统版本。

if(version >= 8.0f){
    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:UIApplicationOpenSettingsURLString]];
}else {
    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"prefs://"]];
}
```