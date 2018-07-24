---
layout: post
title: "如何快速得到数组所有元素累加结果，平均值和最大最小值"
date: 2015-07-08 23:26:59 +0800
comments: true
categories: iOS
---

开发过程中经常会需要数组求和，平均数和最大最小值，第一想法是遍历数组进行累加或者排序。

<!--more-->
其实SDK已经提供了相关的方法，比较特别的是通过**KVC**实现的，示例代码如下：

```objc
	NSArray *values = @[@72, @78, @75, @70, @72, @73, @77, @78, @75, @70, @72, @73, @87, @78, @75, @70, @72];
	NSNumber *avg = [values valueForKeyPath:@"@avg.self"];
	NSNumber *sum = [values valueForKeyPath:@"@sum.self"];
	NSNumber *max = [values valueForKeyPath:@"@max.self"];
    NSNumber *min = [values valueForKeyPath:@"@min.self"];
```

更多内容请查看官方文档:
[https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/KeyValueCoding/Articles/CollectionOperators.html](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/KeyValueCoding/Articles/CollectionOperators.html)