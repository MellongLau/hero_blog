---
title: Swift 3.0之扩展
date: 2016-12-08 22:31:46
tags: swift
---


扩展可以为类，结构体，枚举和协议添加新的功能。包括可以对没有源码访问权限的类型进行扩展。扩展和 Objective-C 分类 的概念类似。（和 Objective-C 的分类不一样的是，Swift 扩展没有名称）。


在 Swift 中，扩展可以做到：

* 添加计算的实例属性和计算的类型属性
* 定义实例方法和类型方法
* 提供新的初始化器
* 定义下标
* 定义并使用新的嵌套类型
* 使现有类型符合协议

值得注意的是：扩展可以为类型添加功能，但是不可以重写现有的功能。

<!-- more -->

## 扩展语法

使用关键字 extension 定义扩展：

```swift
extension SomeType {
    // new functionality to add to SomeType goes here
}
```

扩展可以扩充现有的类型使之可以适应一个或多个协议：

```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // implementation of protocol requirements goes here
}
```

## 计算属性

扩展可以为现有的类型添加计算实例属性和计算类型属性:

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// Prints "One inch is 0.0254 meters"
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// Prints "Three feet is 0.914399970739201 meters"
```

由于这些属性是只读计算属性，所以他们不需要加入关键字 get。

可以直接进行运算：

```swift
let aMarathon = 42.km + 195.m
print("A marathon is \(aMarathon) meters long")
// Prints "A marathon is 42195.0 meters long"
```

值得注意的是：扩展可以添加新的计算属性，但是他们不可以添加存储属性，或者为现有的属性添加属性观察器。


## 初始化器

扩展可以向类添加新的方便初始化器，但是它们不能向类添加新的指定的初始化器或取消初始化器。 指定的初始化器和取消初始化器必须始终由原始类实现提供。

下面定义几个结构体：

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
}
```

我们可以这样来创建 Rect 实例（关于默认初始化器可以查看初始化部分的文章）：

```swift
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
                          size: Size(width: 5.0, height: 5.0))
```

这时，我们可以扩展 Rect 结构体，为其添加新的初始化器：

```swift
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

然后我们就可以使用新的初始化方法来创建实例：

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

## 方法

下面是为Int 类型添加一个叫repetitions的方法：

```swift
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}
```

然后我们可以这样调用这个方法：

```swift
3.repetitions {
    print("Hello!")
}
// Hello!
// Hello!
// Hello!
```

## 变异实例方法

添加了扩展的实例方法也可以修改（或变异）实例本身。 修改self或其属性的结构和枚举方法必须将实例方法标记为`mutating`，就像原始实现中的突变方法一样。

如下面的例子：

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt is now 9
```

## 下标

想实现

* 123456789[0] 返回 9
* 123456789[1] 返回 8

代码如下:

```swift
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
    }
}
746381295[0]
// returns 5
746381295[1]
// returns 9
746381295[2]
// returns 2
746381295[8]
// returns 7
```

如果下标越界，则返回0：

```swift
746381295[9]
// returns 0, as if you had requested:
0746381295[9]
```

## 嵌套类型

扩展添加嵌套类型：

```swift
extension Int {
    enum Kind {
        case negative, zero, positive
    }
    var kind: Kind {
        switch self {
        case 0:
            return .zero
        case let x where x > 0:
            return .positive
        default:
            return .negative
        }
    }
}
```

现在嵌套的类型可以在任何 Int 值中使用：

```swift
func printIntegerKinds(_ numbers: [Int]) {
    for number in numbers {
        switch number.kind {
        case .negative:
            print("- ", terminator: "")
        case .zero:
            print("0 ", terminator: "")
        case .positive:
            print("+ ", terminator: "")
        }
    }
    print("")
}
printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
// Prints "+ + - 0 - 0 + "
```
参考英语原文：
[https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Extensions.html#//apple_ref/doc/uid/TP40014097-CH24-ID151](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Extensions.html#//apple_ref/doc/uid/TP40014097-CH24-ID151)  

*The End*

