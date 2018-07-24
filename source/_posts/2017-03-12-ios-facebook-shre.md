---
title: iOS如何跳转到Facebook指定用户界面
date: 2017-03-12 09:06:45
tags: iOS
---

第一步，先要检测Facebook是否安装，如果安装就直接跳转到app里面指定的用户主页，否则直接用浏览器打开指定的用户主页网页地址。

```objc
BOOL isInstalled = [[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"fb://"]] 
if (isInstalled) {
// 直接跳转到app里面指定的用户主页。
} 
else {
// 用浏览器打开指定的用户主页网页地址。
}
```
 
 
值得注意的是，`iOS9+`需要的`Info.plist`里面加上键名为`LSApplicationQueriesSchemes`加上值：`fb`。

使用下面代码进行跳转：
 
 ```objc
 NSURL *url = [NSURL URLWithString:@"fb://profile/<facebook id>"]; 
 [[UIApplication sharedApplication] openURL:url];
 ```

可以通过这个网站获取到你的Facebook id：[http://findmyfbid.com/](http://findmyfbid.com/)  
 