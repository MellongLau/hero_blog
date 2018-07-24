---
title: Swift 3.0之枚举
date: 2017-03-12 08:36:03
tags: Swift
---

枚举在编程中很多时候要用到，在 Swift 中，枚举具有更多的特性。

### 枚举语法

使用关键字 enum 定义一个枚举

```swift
enum SomeEnumeration {
    // enumeration definition goes here
}
```

例如，指南针有四个方向：

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

这里跟 c 和 objective-c 不一样的是，Swift 的枚举成员在创建的时候没有给予默认的整型值。所以上面代码中的东南西北并不是0到3，相反，不同的枚举类型本身就是完全成熟的值，具有明确定义的**CompassPoint**类型。

<!-- more -->

也可以声明在同一行中：

```swift
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

枚举赋值：

```swift
var directionToHead = CompassPoint.west
```

一旦 **directionToHead** 明确为 **CompassPoint** 类型的变量，后面就可以使用点语法赋值：

```swift
directionToHead = .east
```


### Switch 表达式的枚举值匹配

switch 表达式如下：

```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// Prints "Watch out for penguins"
```

当然这里也可以加上 default 以满足所有的情况：

```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// Prints "Mostly harmless"
```

### 关联值

在 Swift 中，使用枚举来定义一个产品条形码：

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```

可以这样理解上面这段代码：定义一个叫 Barcode 的枚举类型，带有值类型（Int,Int,Int,Int）的 upc和值类型（String）

现在可以这样创建其中一种类型的条形码：

```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

同一产品的另外一个类型的条形码可以这样赋值：

```swift
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

可以用 switch 来查看两种不同的条形码类型：

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

上面的写法也可以改为：

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

### 原始值

这里是一个保存原始 ASCII 值的枚举类型：

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

和上面关联值类似，在枚举中也可以指定每个 case 的默认值（raw values）。

值得注意的是，原始值和关联值不一样，原始值是在第一次定义枚举代码的时候已经设置好的，所有同类型的枚举 case 的原始值都是一样的。而关联值是你创建一个基于枚举 case新的常量或者变量的时候设置的，可以在每次你创建的时候都使用不一样的值。

#### 隐式分配的原始值

如果枚举中 case 的原始值是整型或者字符串的时候，你不需要给每个 case 分配原始值，Swift 会自动帮你分配好值。

例如：

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```
`Planet.mercury` 原始值为1，`Planet.venus` 拥有一个隐式的原始值为2，以此类推。


如果枚举的原始值是 string 类型，那么他的原始值就是 case 名称的文本，例如：

```swift
enum CompassPoint: String {
    case north, south, east, west
}

CompassPoint.south 的隐式原始值是"south", 以此类推。

let earthsOrder = Planet.earth.rawValue
// earthsOrder is 3
 
let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection is "west"
```

#### 从原始值初始化
如果你定义一个原始值类型的枚举，这时枚举会自动创建一个带有原始值类型的初始化器（参数名称为 rawValue），例如：

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet is of type Planet? and equals Planet.uranus
```

不是所有的 Int 值都可以找到对应的 planet，所以原始值初始化器会返回一个 optional 的枚举 case，上面的例子中的 possiblePlanet 是 Planet? 类型。

如果你想找原始值为11的 planet，初始化器将返回 nil：

```swift
let positionToFind = 11
if let somePlanet = Planet(rawValue: positionToFind) {
    switch somePlanet {
    case .earth:
        print("Mostly harmless")
    default:
        print("Not a safe place for humans")
    }
} else {
    print("There isn't a planet at position \(positionToFind)")
}
// Prints "There isn't a planet at position 11"
```

### 递归枚举

 递归枚举是一个包含有一个或多个枚举 case 的关联值枚举实例的枚举，使用关键字 **indirect** 标明某个枚举 case 是递归的。

例如，下面是一个保存简单算法表达式的枚举：

```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

你也可以直接把 **indirect** 写在枚举定义的最前面：

```swift
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

下面的代码是示例如何创建这个递归枚举：

```swift
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
```

应用到计算函数中：

```swift
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}
 
print(evaluate(product))
// Prints "18"
```

上面例子的算法表达式是：**(5 + 4) * 2**，结果为**18**

参考英语原文：
[https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-ID145](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-ID145)