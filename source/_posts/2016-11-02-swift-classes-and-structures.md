---
title: Swift 3.0之类和结构体
date: 2016-11-02 20:41:53
tags: swift
---

### 写在前面

最近在学swift 3.0，主要看的是苹果的官方文档，这里只是根据自己看官方文档的理解所做的一些记录，不是完整的翻译，希望也对你有所帮助。

### 类和结构体区别

Swift的类和结构体具有以下相同的特点：

* 可以定义属性来保存值
* 可以定义方法来提供功能
* 可以定义下标来使用他们的值
* 可以定义初始化器来配置他们的初始化状态
* 可以在默认的实现上扩展他们的功能
* 遵从协议来提供标准的功能

<!-- more -->

类具有结构体没有的额外的功能：

* 继承允许某一个类继承另外一个类的特性
* 类型转换允许你检查并在运行时解释一个类实例的类型
* 析构器允许释放所有该类已经赋值的实例资源
* 引用计数允许多个引用一个类的实例

	结构体一般来说赋值的时候是直接拷贝的，没有使用引用计数的机制。

### 符号定义

下面是一个定义结构体和类的例子：

```swift
struct Resolution {
    var width = 0
    var height = 0
}
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
```

结构体初始化的时候可以直接

```swift
let vga = Resolution(width: 640, height: 480)
```

这点和类不一样，类没有默认的逐个成员的初始化器。

### 结构体和枚举是值类型

```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd
```

再赋值

```swift
cinema.width = 2048
```

结果

```swift
print("cinema is now \(cinema.width) pixels wide")
// Prints "cinema is now 2048 pixels wide"
```

然而hd.width还是1920

```swift
print("hd is still \(hd.width) pixels wide")
// Prints "hd is still 1920 pixels wide"
```

可见赋值过程是做了一次深度拷贝。

枚举也是具有同样的行为, 如以下例子，`rememberedDirection`的值并没有改变：

```swift
enum CompassPoint {
    case north, south, east, west
}
var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection
currentDirection = .east
if rememberedDirection == .west {
    print("The remembered direction is still .west")
}
// Prints "The remembered direction is still .west"
```

### 类是引用类型

例如：

```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```

进行赋值引用

```swift
let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0
```

结果

```swift
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// Prints "The frameRate property of tenEighty is now 30.0"
```

### 标识符

* 完全相同（===）
* 不完全相同（!===）

```swift
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```

完全相同（===）和等于（==）是不一样的：

* 完全相同意思是两个类类型的常量或者变量指向完全相同的类实例
* 等于意思是两个实例被认为值相同或者相等, 可以自行定义==操作符来进行判断两个实例在某种意义上是相等的

### 选择使用类和结构体

由于结构体的实例一般是值传递，而类实例一般是引用传递，因此你需要根据实际情况来考虑应该定义一个类还是结构体.

如有以下一种或多仲情况使用结构体：

* 结构体主要的目的是封装少量的相关性简单数据值
* 在结构体的实例赋值或者传递的时候，需要考虑到封装好的值会被拷贝而不是引用是否是合理的
* 任何保存于结构体的属性都是值类型的，他们也是期望被赋值或者传递时是拷贝而不是引用
* 结构体不需要从其他存在的类型继承属性或者行为

看看几个使用结构体恰当的例子：

* 几何图形的大小，可以封装width和height属性，都是Double类型
* 指向连续序列范围的方法，可以封装start和length属性，都是Int类型
* 一个在3D坐标系统的点， 可以封装x, y和z属性，都是Double类型

其他的情况请定义类并创建类实例，管理和传递都使用引用。
在实践中，大部分的自定义数据结构都是使用类居多，很少使用结构体。


### String、Array和Dictionary的赋值和拷贝行为

String, Array和 Dictionary都是结构体，因此赋值直接是拷贝，而NSString, NSArray 和NSDictionary则是类，所以是使用引用的方式。

参考英语原文：
[https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-ID82](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-ID82)