---
layout: post
title: "更改navigationController push和pop界面切换动画"
date: 2012-05-22 09:52 +0800
comments: true
categories: iOS
---


有时候我们需要自定义`navigationController` `push`和`pop`界面切换动画，用到的代码如下：

- For Push:

```objc
MainView *nextView=[[MainView alloc] init];  
[UIView  beginAnimations:nil context:NULL];  
[UIView setAnimationCurve:UIViewAnimationCurveEaseInOut];  
[UIView setAnimationDuration:0.75];  
[self.navigationController pushViewController:nextView animated:NO];  
[UIView setAnimationTransition:UIViewAnimationTransitionFlipFromRight forView:self.navigationController.view cache:NO];  
[UIView commitAnimations];  
[nextView release];  
```

- For Pop:

方法一：

```objc
[UIView  beginAnimations:nil context:NULL];  
[UIView setAnimationCurve:UIViewAnimationCurveEaseInOut];  
[UIView setAnimationDuration:0.75];  
[UIView setAnimationTransition:UIViewAnimationTransitionFlipFromLeft forView:self.navigationController.view cache:NO];  
[UIView commitAnimations];  
  
[UIView beginAnimations:nil context:NULL];  
[UIView setAnimationDelay:0.375];  
[self.navigationController popViewControllerAnimated:NO];  
[UIView commitAnimations];  
```

方法二：

可实现左右滑动动画，可设置滑动方向。

```objc
CATransition* transition = [CATransition animation];  
transition.duration = 0.5;  
transition.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];  
transition.type = kCATransitionFade; //kCATransitionMoveIn; //, kCATransitionPush, kCATransitionReveal, kCATransitionFade  
//transition.subtype = kCATransitionFromTop; //kCATransitionFromLeft, kCATransitionFromRight, kCATransitionFromTop, kCATransitionFromBottom  
[self.navigationController.view.layer addAnimation:transition forKey:nil];  
[[self navigationController] popViewControllerAnimated:NO];  
```

具体的动画参数请自行更改。