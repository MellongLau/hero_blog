---
title: Swift 中定义单例
date: 2018-03-07 10:13:50
tags: [swift, ios] 
---

## 什么是单例

单例模式（Singleton Pattern），也叫单子模式，是一种常用的软件设计模式。 在应用这个模式时，单例对象的类必须保证只有一个实例存在。

单实例Singleton设计模式可能是被讨论和使用的最广泛的一个设计模式了，这可能也是面试中问得最多的一个设计模式了。这个设计模式主要目的是想在整个系统中只能出现一个类的实例。这样做当然是有必然的，比如你的软件的全局配置信息，或者是一个Factory，或是一个主控类，等等。

## 如何在 swift 中创建单例

<!-- more -->

在 swift 中有以下这两种方式可以创建单例

* 全局变量的方式


```swift
let sharedNetworkManager = NetworkManager(baseURL: API.baseURL)

class NetworkManager {

    // MARK: - Properties

    let baseURL: URL

    // Initialization

    init(baseURL: URL) {
        self.baseURL = baseURL
    }

}
```

使用该全局变量进行引用

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    print(sharedNetworkManager)
    return true
}
```

* 静态属性及私有化构造方法的方式

```swift
class NetworkManager {

    // MARK: - Properties

    private static var sharedNetworkManager: NetworkManager = {
        let networkManager = NetworkManager(baseURL: API.baseURL)

        // Configuration
        // ...

        return networkManager
    }()

    // MARK: -

    let baseURL: URL

    // Initialization

    private init(baseURL: URL) {
        self.baseURL = baseURL
    }

    // MARK: - Accessors

    class func shared() -> NetworkManager {
        return sharedNetworkManager
    }

}
```

直接调用类方法进行引用

```swift
NetworkManager.shared()
```

参考自： [What Is a Singleton and How To Create One In Swift](https://cocoacasts.com/what-is-a-singleton-and-how-to-create-one-in-swift/)

*The End*

