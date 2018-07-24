---
layout: post
title: "制作树莓派wifi遥控和自动避障小车"
date: 2015-07-12 22:16:06 +0800
comments: true
categories: pi
tags: [树莓派, 自动避障, 小车]
---


## 写在前面
----
前几年买了树莓派，当时主要是想用来做客厅多媒体盒，不过实际使用下来有点卡，速度不尽人意。虽然后来换了class10的SD卡速度有点提升，但也很少拿去看电影了，多数时间是用来做下载机。

其实之前也有听说可以做很多有趣的东西，我最感兴趣就是做个遥控小车，一直有这个想法，就是没有实际行动。最近在网上又看到类似的遥控车，突然间又来了兴致，这次决心很大，立即就找了下资料，并且在淘宝上买了配件。趁着周末有时间，花了半天时间组装好了，又花了半天改了程序支持自动避障，还做了个iPad/iPhone的手机端遥控app。自己又是摆道具又是录视频的，忙得不亦乐乎。废话少说，现在进入正题！

## 视频效果

这是后来做的加了三路寻轨功能的视频，后面文章会继续介绍：
<embed src="http://player.youku.com/player.php/sid/XMTI4Nzk0NjMyOA==/v.swf" allowFullScreen="true" quality="high" width="480" height="400" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed>

<!-- more -->

## 需要的材料
===
1. `树莓派小车底盘`，这个上淘宝一搜一大堆，我买的四驱的，有带亚克力两层车板。
2. `三种杜邦线`，每一种买十条就可以了，我当时怕不够用，每一种都买了20条。
3. `移动电源`，这个家里之前有一个飞毛腿的，要买双usb输出的。
4. `树莓派`
5. `L298N电机驱动板模块`
6. `红外避障模块和超声波测距模块`，如果想做避障功能就需要买这个传感器，买两个（开始的时候不知道，我只买了一个红外避障模块...）。
7. `USB无线网卡`

上面的花费下来，不计树莓派、USB无线网卡和移动电源，大概花了90元左右。

## 材料组装
===

### 小车底盘
材料到手之后，先组装车底盘，安装说明书把第一层组装好，马达连线见下图，线我是用杜邦线一分为二去接的。

![image](/blogImages/car_01.jpg)

注意走线是上下交叉连接。

### 与L298N驱动模块连线

模块两边有各有两个out接口分布连接两边马达，四个IN接口连接树莓派的四个GPIO接口，连上后记得自己连接的接口编号就可以，写代码的时候需要。

### 供电

我使用之前买的移动电源供电，自带有两个USB接口，一个接树莓派，一个接L298N驱动模块。也可以自己买电池盒进行串联供电。
接模块的电源我使用usb线剪的，其中黑线是接地，红线接VCC。

## 遥控程序
===

遥控主要是通过树莓派的GPIO设置高低电平信号来控制小车前进、后退、左转、右转和停止，具体可以参考代码和下面GPIO的接口说明。值得注意的是，1对应的树莓派电路板背面焊锡为方形的针脚。

![image](/blogImages/pi_GPIO.png)

### web版
web版可以直接使用王恒的版本：
[https://github.com/wujiwh/piCar](https://github.com/wujiwh/piCar)

主要修改自己接的GPIO接口和对应的方向。

### iOS版
简单写了iOS客户端，服务端是用王恒的版本改的：
[RaspiCar](https://github.com/MellongLau/RaspiCar)

### 自动避障版
由于只买了一个红外避障模块，于是只能单边避障，这里是代码：

```python
#!/user/bin/env python
 
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
GPIO.setwarnings(False)
GPIO.setup(11,GPIO.OUT)
GPIO.setup(12,GPIO.OUT)
GPIO.setup(15,GPIO.OUT)
GPIO.setup(16,GPIO.OUT)
GPIO.setup(7,GPIO.IN)

def t_stop():
	GPIO.output(11, False)
	GPIO.output(12, False)
	GPIO.output(15, False)
	GPIO.output(16, False)

def t_down():
	GPIO.output(11, True)
	GPIO.output(12, False)
	GPIO.output(15, True)
	GPIO.output(16, False)

def t_up():
	GPIO.output(11, False)
	GPIO.output(12, True)
	GPIO.output(15, False)
	GPIO.output(16, True)

def t_right():
	GPIO.output(11, False)
	GPIO.output(12, True)
	GPIO.output(15, True)
	GPIO.output(16, False)

def t_left():
	GPIO.output(11, True)
	GPIO.output(12, False)
	GPIO.output(15, False)
	GPIO.output(16, True)


while True:
    in_right= GPIO.input(7)
    if in_right == False:
    	t_left()
    else:
   		t_up()
```



## 最后
====
完成效果图：

![image](/blogImages/car_02.jpg)
