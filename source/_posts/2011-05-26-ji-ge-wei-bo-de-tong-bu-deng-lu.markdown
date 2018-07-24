---
layout: post
title: "几个微博的同步——登录"
date: 2011-05-26 14:59:40 +0800
comments: true
tags: [webbrowser, 加密, firebug, 腾讯qq, 算法]
---

最近做了个微博软件，可以直接通过软件发微博，用了一段时间后，开通了腾讯微博，于是想把之前的微博内容也同步到腾讯微博上。马上用firebug分析了一下腾讯微博的登录过程，果然发现腾讯又对发送的qq密码进行加密，所以，要想顺利登录腾讯微博，第一步要做的就是要加密算法拿到手。

既然发送qq密码之前已经进行了加密，那加密肯定是在客户端进行的，于是我直接用firebug在相关js里面进行搜索，很快就定位到了加密算法的js文件，在该js文件可以找到对qq密码进行加密的代码：

```js
if (E[A].name == "p") {
var F = "";
F += E.verifycode.value;
F = F.toUpperCase();
B += md5(md5_3(E.p.value) + F)
} else {
if (E[A].name == "u1" || E[A].name == "ep") {
B += encodeURIComponent(E[A].value)
} else {
B += E[A].value
}
}
```

分析了一下加密主要是这部分：`B += md5(md5_3(E.p.value) + F)，E.p.value`就是输入的密码，B是发送的参数，F是获取到的验证码。知道了加密过程，接下来的就好办了。直接用MSScriptControl来调用md5和md5_3函数对qq密码进行加密并且发送，当然在此之前比较先获取验证码。

对qq密码加密后，接着就直接根据firebug的分析，把相关数据进行发送就可以了，在发送之前需要注意的是记得要先获取并保存到相关的cookie，不然也会提示参数错误。

其实登录还有一个方法，就是通过webBrowser控件，直接定位到腾讯微博的登录窗口，在此之前开了qq的话就直接可以按快速登录进行登录了，然后通过webBrowser直接获取登录后的cookie就可以了。