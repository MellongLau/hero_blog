---
layout: post
title: "如何画曲线图"
date: 2015-07-09 10:53:29 +0800
comments: true
categories: iOS
---


效果：

![image](/blogImages/curve_graph.png)

由左下图可以看到两点之间有两个control point，可以取两点之间的中点的x作为两个control point的x，前一点的y作为control point1的y值，后一点的y作为control point2的y值。

<!--more-->

![image](/blogImages/curve_controlpoint.png)

代码：

```objc
	- (void)drawRect:(CGRect)rect
	{
	    CGContextRef context = UIGraphicsGetCurrentContext();
	    CGContextSetRGBStrokeColor(context,1,0,0,0.6);
	    CGContextSetLineWidth(context, 1);
	   
	    CGContextScaleCTM(context, 1, -1);
	    CGContextTranslateCTM(context, 0, -rect.size.height);
	   
	    NSArray *values = @[@50, @65, @75, @70, @80, @90, @67, @65, @75, @70, @80, @90, @67, @65, @75, @70, @80, @90, @67]; // y值数组
	    CGFloat space = 10.f; //x坐标间隔
	    CGFloat startX = 10.f; //x坐标起始点
	    int i = 0;
	    for (int x = startX; x < rect.size.width-2*startX; x+=space) {
	        if (i >= values.count) {
	            break;
	        }
	       
	        CGFloat currentY = [values[i] floatValue];
	        if (x == startX) {
	            CGContextMoveToPoint(context, x, currentY);
	        }else{
	           
	            CGFloat preY = [values[i-1] floatValue];
	            CGFloat controlPointX = x-5;
	            CGContextAddCurveToPoint(context, controlPointX, preY, controlPointX, currentY, x, currentY);
	        }
	        i++;
	    }
	    CGContextDrawPath(context, kCGPathStroke);
	}
```