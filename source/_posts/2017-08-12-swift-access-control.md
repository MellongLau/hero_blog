---
title: Swift 之访问控制
date: 2017-08-12 10:13:50
tags: [iOS, swift] 
---

访问控制对访问你的其他代码源文件和模块部分进行了约束。这个特性允许你隐藏你的代码实现，并且指定通过其可以访问和使用该代码的优选接口。

class，structure 和 enumeration 都可以指定访问级别，当然，property，method，initializer 和 属于这里类型的 subscript。protocol 可以限制到某个上下文，全局变量，变量和函数也可以。

另外，Swift 也提供默认的使用级别给典型的使用场景。确实，如果你编写一款单一目标的 app，你可能根本不需要明确地指定访问控制级别。

<!-- more -->

### 模块和源文件

Swift的访问控制模型是基于模块和源文件的概念。

一个模块是单个的代码分布单元————一个 framework 或者应用程序是作为单个单元编译和传递的，他们能够通过 Swift 的 import 关键字被其他模块导入。

在Swift 中， Xcode的每一个 build target（如 一个 app bundle 或者 framework）被当成一个单独的模块。

虽然通常做法是在不同的源文件定义不同的类型，然而一个源文件事实上可以包含不同的类型，函数等的定义。

### 访问级别

Swift 为你的代码实体提供5个不同的访问级别：

* Open 访问和 public 访问允许实体能够被使用在任何来自起决定作用的模块的源文件，或者来自于其他被导入的模块的源文件。通常使用 open 或者 public 来指定framework 的公开接口。两者的不同点将在下面进行描述。

* Internal 访问允许实体被使用在他们定义模型的任何源文件里面，但是不能在模块外部的任何源文件使用。通常在定义一个 app 或者一个 framework 的内部结构的时候使用 internal 访问。

* File-private 访问限制了在定义源文件中实体的使用。使用 file-private 访问来隐藏特定功能的实现细节，当这些细节在整个文件中使用的时候。

* Private 访问将实体的使用限制在封闭声明中。使用 private 访问来隐藏特定功能的实现细节，当这些细节在单个声明使用时。

Open 访问是最高访问级别，private 是最低访问级别（最大限制性）。

Open 访问只用在类和类成员，他和 publick 访问的区别如下：

* 使用 public 访问的类， 或者其他更多限制性的访问级别，只能在定义的模块内创建子类。

* 使用 public 访问的类成员，或者其他更多限制性的访问级别，只能在定义的模块内被其子类重写。

* Open 类可以被定义的模块或者其他 import 该模块的地方创建子类。

* Open 类成员可以被定义的模块或者其他 import 该模块的地方创建的子类重写。


>> 简单来说就是 public 和 open 的区别就是public 比 open 少了模块外的类继承和类成员重写的权限。

### 访问级别的指导原则

在 Swift 中，访问级别遵从总的指导原则是：没有实体可以被定义在另外一个拥有较低访问级别（更多限制）的实体之内。

例如：

* public变量不能被定义为具有internal, file-private或者 private 类型，因为这种类型可能不能用在使用公共变量的任何地方。

* 函数不能具有比其他参数类型和返回类型更高的访问级别，因为该函数可以在其组成类型不可被周围代码使用的情况下使用。

下面会有更详细的介绍。

### 默认的访问级别

如果你自己没有指定一个明确的访问级别，所有代码中的实体都有一个默认的internal访问级别。结果，在很多情况下你不需要对你的代码指定明确的访问级别。

### 单目标应用程序的访问级别

 如果你写的是一个 i 简单的单目标应该程序，那么你的程序代码就是典型的自包含程序，并不需要在程序模块的外部进行使用。默认的访问级别 internal 已经满足这个需求。因此，你不需要去指定一个访问级别。然而，你可能需要把你部分的代码标记为文件私有或者私有，从而使得在程序模块中的其他代码隐藏他们的实现细节。

### Frameworks 的访问级别

 当你开发一个 framework，标记 open 或者 public 以便它能够被其他模块访问到，例如某个程序引入这个 framework 的时候。这个面向公众的接口是framework 的程序编程接口（或者 API）。

值得注意的是：任何 framework 的内部实现细节都还可以使用默认的内部访问级别，或者可以标识为私有或者文件私有级别，如果你想对framework 的其他部分内部代码隐藏他们的话。只有当你想让一个实体成为你的 framework 的 API 的一部分的话，那么你就需要把这个实体标识为 open 或者 public。

### 单元测试目标的访问级别

当你写的是一个包含单元测试目标的程序时，那么你需要让你程序中的代码可以被测试模块使用到以便于测试。一般情况下，只有被标识为 open 或者 public 的实体才可以被其他模块访问到。然而，如果你把产品的模块 import 声明前加入 @testable 属性并且在打开测试选项下编译产品模块的话，那么单元测试目标就能够访问任何的 internal 实体。

### 访问级别语法

为实体定义访问级别：

```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
fileprivate class SomeFilePrivateClass {}
private class SomePrivateClass {}
 
public var somePublicVariable = 0
internal let someInternalConstant = 0
fileprivate func someFilePrivateFunction() {}
private func somePrivateFunction() {}
```

除非有其他的指定，否则的话默认的访问基本是 internal，这也就意味着 SomeInternalClass 和 someInternalConstant 能够在不明确访问级别修饰符的情况下也还拥有 internal 的访问级别：

```swift
class SomeInternalClass {}              //  隐式 internal
let someInternalConstant = 0            //  隐式 internal
```

翻译自：[https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AccessControl.html#//apple_ref/doc/uid/TP40014097-CH41-ID3](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AccessControl.html#//apple_ref/doc/uid/TP40014097-CH41-ID3)

*The End*

