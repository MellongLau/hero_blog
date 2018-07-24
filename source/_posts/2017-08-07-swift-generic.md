---
title: Swift 之泛型 
date: 2017-08-07 10:13:50
tags: [iOS, swift] 
---

泛型代码允许你编写灵活，可以重用适用于任何类型的函数和类型。
大部分的Swift标准库都是用泛型代码编写。Array和Dictionary事实上都是泛型集合。你可以创建Int值的数组，或者String值的数组，以及其他swift类型的数组。Dictionary也是类似的。


<!-- more -->

一般情况我我们这样写函数

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

使用in-out关键字的参数可以修改外部传入的变量的值

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```

可以看到外部定义的变量经过`swapTwoInts(_:_:)`函数交换了值。这里我们直接定义的是Int类型的值交换，如果我们想交换String或者Double的类型的值，那我们还得重新写对应类型的函数：

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}
 
func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

从上面的代码可以看到，这几个函数的代码除了类型不一样之外，其他都是一样的，那么这时我们可以用泛型来进行定义。


### Generic Functions

泛型函数可以适配任何类型。

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

看看和上面指定类型写法的区别

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

泛型函数定义好之后，我们就可以直接调用

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt is now 107, and anotherInt is now 3
 
var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString is now "world", and anotherString is now "hello"
```

上面这个函数只是为了举例说明泛型的用法，实际使用中我们可以直接使用Swift已经存在的函数`swap(_:_:)`来交换两个变量的值。

### 类型参数

你可以在尖括号内写多个类型参数名称, 用逗号隔开。函数被调用的时候，这些类型参数将会被替换成实际的类型。

### 类型参数的命名

可以采用类似Dictionary<Key, Value>, 或者Array<Element>的命名方式，如果类型参数跟实际类型没有多大搞关联，也可以使用单个字母T, U, 和 V，例如swapTwoValues(_:_:)就是用的 T 作为类型参数。

另外，类型参数命名的**首字母要大写**，例如 T 和 MyTypeParameter，这是为了表明这些是类型的占位符，而不是一个值。

### 泛型类型

在Swift中，我们可以自己定义泛型类型。自定义类，结构体和枚举都可以使用和数组，字典类似的任何类型。

这部分主要展示给你是一个实现泛型集合类型的栈，UINavigationController 中就有使用到栈的概念。

结合下面图例更容易理解：

![](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Art/stackPushPop_2x.png)

下面是没有使用泛型版本的栈：


```swift
struct IntStack {
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}

```

当然，只支持 Int 的栈并不能满足我们的项目需求，所以下面是使用泛型版本：

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

这里使用泛型参数**Element**代替实际类型Int, 使用尖括号<Element>写在结构名称后面。后面所有地方都可以用Element表示类型，

使用的时候只需在初始化的时候写 Stack<String>(), 则表明这个是一个字符串栈:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// the stack now contains 4 strings
```

具体才栈操作相信大家已经非常熟悉了，这里就不再赘述。

### 扩展泛型类型

下面是一个扩展泛型栈类型的例子：

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

这里为栈添加了一个只读的 topItem 属性。

topItem 这个计算属性现在可以在任何的栈实例中被使用，可以用它来获取栈的顶部元素，但不移除这个元素。

```swift
if let topItem = stackOfStrings.topItem {
    print("The top item on the stack is \(topItem).")
}
// Prints "The top item on the stack is tres."
```

### 类型约束

有些时候我们需要给泛型类型加上约束，例如指定类型参数必须继承某个指定的类，或者遵循某个协议或者协议组合。

例如Dictionary类型对 key 做了限制， key 必须是遵循 Hashable 协议的。其中，Swift 的基本类型（String，Int，Double和 Bool）默认都是遵循 Hashable 协议的。

在创建自定义泛型类型的时候，你也可以定义你自己的类型约束。

### 类型约束符号

如下例所示，为类型参数添加类型约束的时候，可以在类型参数的名称后面加入一个类或者协议的约束，多个类型参数可用逗号隔开：

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```

上面例子中中，第一个类型参数 T 的类型约束是：T 必须是 SomeClass 的子类。第二个类型参数 U 则必须遵循协议 SomeProtocol。

### 类型约束实践

这里有一个非泛型函数叫 findIndex(ofString:in:)，这个函数提供一个 String 值来查找一个String 数组。findIndex(ofString:in:)函数返回一个 optional Int 值，返回匹配的字符串在数组中的位置，如果不存在这个字符串则返回 nil：

```swift
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

findIndex(ofString:in:)函数可以用来查找数组中的字符串：


```swift
let strings = ["cat", "dog", "llama", "parakeet", "terrapin"]
if let foundIndex = findIndex(ofString: "llama", in: strings) {
    print("The index of llama is \(foundIndex)")
}
// Prints "The index of llama is 2"
```

然而，只能查找数组中字符串值位置的理念不是很好用。你可以通过替换任何字符串为类型 T 来代替，写一个相同功能的泛型函数。

这里我们把findIndex(ofString:in:) 修改为泛型版本 findIndex(of:in:)。值得注意的是，返回类型依然是 Int?，因为函数返回一个optional 索引数字，而不是 来自数组的 optional 值。下面这个函数不能编译，后面会解释原因：

```swift
func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

 上面所写的这个函数不能编译。问题在于相等性的检查，"if value == valueToFind"。不是任何类型在 Swift 中都可以使用操作符（==）进行相等比较。例如，如果你创建你自己的类或者结构体来展现一个复杂的数据模型，那么对于这个类或结构体的"equal to" 的含义不是 Swift 能够帮你猜到的。正因为如此，不可能保证这些代码对于所有可能的类型 T 都可行，同时当你试图编译这些代码的时候有提示相应的错误。

但是，你还没有失去一切。Swift 标准库定义一个协议叫 Equatable，这个协议要求所有遵循的类型要实现相等操作符（==）和不等操作符（!=）来比较两个不同类型的值。所有的 Swift 标准类型都自动支持 Equatable protocol。

任何是 Equatable 的类型都能够安全地在findIndex(of:in:)函数中使用，因为他保证支持相等操作符。为了验证这个事实，当你定义函数的时候，你可以写一个类型约束于 Equatable 的类型参数定义：

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

findIndex(of:in:)的单一类型参数写作 T: Equatable，这样写的意思是"任何遵循于协议 Equatable 的类型 T"。

findIndex(of:in:)函数现在可以成功编译，并能够被使用任何是 Equatable 的类型，如 Double 和 String：

```swift
let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
// doubleIndex is an optional Int with no value, because 9.3 is not in the array
let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])
// stringIndex is an optional Int containing a value of 2
```

### 关联类型

当定义一个协议的时候，有时候声明一个或多个关联类型作为协议定义的一部分是很有用的。一个关联类型提供一个类型的占位名，这个类型被用作协议的一部分。用于关联类型的实际类型直到协议被采用时才被指定。关联类型使用associatedtype关键字指定。

#### 关联类型实践

这里是一个声明了一个关联类型叫 ItemType 名字为 Container 的协议示例：

```swift
protocol Container {
    associatedtype ItemType
    mutating func append(_ item: ItemType)
    var count: Int { get }
    subscript(i: Int) -> ItemType { get }
}
```

协议 Container 定义了任何容器必须提供的三个要求的功能：

* 必须可以使用 append(_:)方法添加一个新的项目。
* 必须可以通过使用一个返回 Int 值的 count 属性来访问容器中的项目数。
* 必须可以通过一个 Int 索引值的下标来获取容其中每个项目。

这个协议并没有指明容器中应该保存多少项目或者允许什么类型保存。这个协议只是指明任何看作为 Container 的类型必须提供这三种功能。一个符合的类型可以提供额外的功能，只要它满足这个三个要求就可以了。

Container 协议声明一个关联类型叫 ItemType，写作 associatedtype ItemType。这个协议没有定义 ItemType 是什么，这个信息留给任何符合的类型来提供。尽管如此，ItemType 别名提供一个方法在 Container 中引用项目的类型，并且定义一个类型给 append(_:)方法和下标使用，从而确保任何 Container 期望的行为被执行。

翻译自：
[https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html#//apple_ref/doc/uid/TP40014097-CH26-ID179](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html#//apple_ref/doc/uid/TP40014097-CH26-ID179)

*The End*

