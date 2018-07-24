---
title: 迁移到swift3.0有用的技巧
date: 2016-11-10 23:09:02
tags: swift
---


不久之前swift3.0发布了，新版本可以在Xcode 8中使用了，或者你可以直接从swift.org下载编译器。

从代码可读性来看，新版本有很多提升，函数调用的连续性，更好的命名约定和移除了部分c风格的元素。

从代码可读性来来看，`NS`前缀已经从`Foundation`类型中移除，例如`NSBundle.mainBundle()`现在改为`Bundle.mainBundle()`.

<!-- more -->

c风格的一元操作符`++`和`--`在3.0中已经不适用了：

```swift
// Only in Swift 2.2 and earlier
var number = 10  
number++  
++number
number--  
--number
```

相对应的表达方式是`number += 1` or `number -= 1`.

另外一个有趣的变化是移除了c风格的`for-loop`,我记得在学校中（使用c语言）写这种循环：

```swift
// Only in Swift 2.2 and earlier
let steps = 5  
for var step = 0; step < steps; step++ {  
  print(step)
}
// 0 1 2 3 4
```

主要的原因是存在了更好的对应写循环的方法`for-in`和`stride()`.`for-loop`在理解上比较困难和缺少`swift`风格。当新的方法出现后，`for-loop`已经很少在代码库中使用了。

这篇文章将讲解典型的`for-loop`使用场景，同时说明迁移到 `for-in, stride()`或者简单的`while() {}`.

### 1. 如何迁移 for-loop 到 for-in
`for-loop` 应用的典型场景在一个数字区间内迭代。这些数字可以是数组的索引等.

例如，让我们遍历数组的每一个元素：

```swift
// Only in Swift 2.3 and earlier
let birds = ["pigeon", "sparrow", "titmouse"]  
for var index = 0; index < birds.count; index++ {  
  print(birds[index])
}
// "pigeon" "sparrow" "titmouse"
```

可见，`let index = 0; index < birds.count; index++` 的循环部分是冗长的。许多元素是多余的，整个表达式可以简化的。替换为手动的增量，整个操作可以用更具表达性的语法来自动化。

![Image](https://rainsoft.io/content/images/2016/09/1-1.png)

`for-in` 循环更简短和更具表达性。让我们迁移上面的代码：

```swift
let birds = ['pigeon', 'sparrow', 'titmouse']  
for index in 0..<birds.count {  
  print(birds[index])
}
// 'pigeon', 'sparrow', 'titmouse'
```


现在感觉好很多了。`index in 0..<birds.count` 更容易阅读和理解。`0..<birds.count` 这部分定义一个半开区间的 Range 类型。for-in 循环迭代0，1和2的范围值（不包括上限3）。

这不是全部！ 你甚至可以跳过索引并直接访问数组元素：

```swift
let birds = ["pigeon", "sparrow", "titmouse"]  
for bird in birds {  
  print(bird)
}
// "pigeon" "sparrow" "titmouse"
```

可以看出，对于标准数组或集合迭代`for-in`对于`for-loop`是一个更好的替代。 至少在这种情况下，在Swift 3.0中删除`for-loop`的决定是合理的。

### 2. 如何迁移 for-loop 到 stride
你可以合理地要求for-loop虽然是冗长的，但仍然是灵活的。 它对于更复杂的迭代是有用的。

让我们尝试一个场景。你要打印一个具有奇数索引元素的元素数组。一个 for-loop 可能看起来像这样：

```swift
// Only in Swift 2.3 and earlier
let colors = ["blue", "green", "red", "white", "black"]  
for var index = 0; index < colors.count; index += 2 {  
  print(colors[index])
}
// => "blue" "red" "black"
```

由于索引根据表达式 index += 2而每次增加2，所以只有奇数索引的元素会被显示："blue", "red" and "black".

你可以尝试使用 for-in 并定义一个范围。但是需要奇数索引加上附加的验证：

```swift
let colors = ["blue", "green", "red", "white", "black"]  
for index in 0..<colors.count {  
  if (index % 2 == 0) {
    print(colors[index])
  }
}
// => "blue" "red" "black"
```

的确， `if (index % 2 == 0) { ... }` 条件句在这里看起来怎么样。

这种情况很符合使用 Swift 的stride(from: value, to: value, by: value)函数。定义开始，结束（不包括上限）和步长值，函数返回相应的数字序列。

让我们在我们的场景中应用stride：

```swift
let colors = ["blue", "green", "red", "white", "black"]  
for index in stride(from: 0, to: colors.count, by: 2) {  
  print(colors[index])
}
// => "blue" "red" "black"
```

`stride(from: 0, to: colors.count, by: 2)` 返回以0开始到5的数字（上限不包含5），步长为2。对于 for-loop，这是一个好的替代。

如果上限必须包含进来，这里有另外一种函数格式：
`stride(from: value, through: value, by: value)`。第二个参数的标签是 `through`， 这个标签是用以指明是包含上限的。

### 3. 其他情况坚持使用while
c风格for-loop的每个组件都有一个很好的属性：初始化，跳出严重和完全可配置的增量：

```swift
for <initialization>; <verification>; <increment> {  
  // loop body
}
```

此外，你可以省略其中的任何组件，要是你能在for-loop的循环块打破循环。

例如，让我们打印一个数字数组的元素，直到数字0被遇到。可以使用C风格的for-loop:

```swift
// Only in Swift 2.2 and earlier
let numbers = [1, 6, 2, 0, 7], nCount = numbers.count  
for var index = 0; index < nCount && numbers[index] != 0; index++ {  
  print(numbers[index])
}
// => 1 6 2
```

验证部分 `index < nCount && numbers[index] != 0` 是用以检查是否0在数组中出现。如果出现，则跳出循环。  
所以只有0之前的数字被打印出来：1，6和2。

`for var index in 0..<nCount` 是一个迁移选项。你只是需要使用条件 `if numbers[index] == 0` ，当0出现的时候跳出循环：

```swift
let numbers = [1, 6, 2, 0, 7], nCount = numbers.count  
for index in 0..<nCount {  
  if (numbers[index] == 0) {
    break
  }
  print(numbers[index])
}
// => 1 6 2 
```

但 break 语句出现，它会轻微减少阅读流程。但是我想要容易地阅读代码流程！

`while(<condition>) {...}` 循环可能是一个更好的替代方案。让我们看看上一个例子是如果被修改的：

```swift
let numbers = [1, 6, 2, 0, 7], nCount = numbers.count  
var index = 0  
while (index < nCount && numbers[index] != 0) {  
  print(numbers[index])
  index += 1
}
// => 1 6 2
```

如果你有的情况无法使用 for-in 或者 `stride()`, 那么我推荐你使用 `while(){}`。

### 4. 统一参数标签行为
在Swift 2.2 和更早版本你可以在调用函数的时候忽略第一个参数的标签：

```swift
// Only in Swift 2.2 and earlier
func sum(firstItem: Int, secondItem: Int) -> Int {  
  return firstItem + secondItem
}
sum(5, secondItem: 2) // => 7
```

对于我来说，这个忽略的做法给我带来困扰。你不得不忽略第一个参数的标签，然而剩下的参数却还保持有标签。这是一种不自然的规则。

![Image](https://rainsoft.io/content/images/2016/09/2-2.png)

幸运的是从3.0版本开始，所有参数将强制拥有标签。
让我们来迁移上一个例子：

```swift
func sum(firstItem: Int, secondItem: Int) -> Int {  
  return firstItem + secondItem
}
sum(firstItem: 5, secondItem: 2) // => 7  
```

`myFun(firstParam: 1, secondParam: 2)`看起来更好。你知道严格的参数含义。简单，一致和清晰的方式。


如果你因为某些原因想在Swift 3.0中调用函数的时候忽略第一个标签，使用_ 作为那个参数的参数标签：

```swift
func sum(_ firstItem: Int, secondItem: Int) -> Int {  
  return firstItem + secondItem
}
sum(5, secondItem: 2) // => 7 
```

然而从长远来看我不推荐这种做法。他破坏了Swift代码中函数/方法调用的一致性。

Swift 命名向导 有很多有用的命名方面的建议。

### 5. 总结

Swift 3.0 做了一个很好的修改列表。其中大部分是重大的修改，所以你必须花些功夫来迁移Swift 2.3或者更旧的代码。

Swift 的制造者花了很多功夫来使这门语言用起来尽可能的舒服。
有时候，这个过程产生重大更改。幸运的是相比提高代码的可读性和跨语言语法的一致性来说，这是一个相对小的代价。

C风格的元素如for-loop，一元增量和减量运算被删除。对于这些结构Swift提供了更好的选择。
例如C语言风格的for循环很容易由for-in所取代。你可以使用`stride()`函数来进行更多可配置的迭代。

我最喜欢的改进是Swift 3.0引入了函数参数标签的一致性和清晰度。简单易记的规则：始终指明参数的标签。

我建议你也访问Swift 3.0官方的迁移向导。


> *英文原文链接：[https://rainsoft.io/useful-tips-for-migrating-to-swift-3-0/](https://rainsoft.io/useful-tips-for-migrating-to-swift-3-0/)*
