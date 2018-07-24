---
title: 迁移到Swift 3.0之@discardableResult
date: 2016-10-25 23:42:40
tags: Swift
---

在Swift 2.x的时候，带返回的方法我们如果在调用的时候后面使用到返回的参数，编译器不会有任何的警告，想要编译器给出警告的话需要自己在方法前面添加属性`@warn_unused_result`, 如

```swift
@warn_unused_result func doSomething() -> Bool {
  return true
}
```

这时候调用这个方法没有使用返回参数的情况下编译器就会给出警告：

>> Result of call to 'doSomething()' is unused

到了 Swift 3.0 我们不需要这样写了，默认情况下编译器就是会去检查返回参数是否有被使用，没有的话就会给出警告。如果你不想要这个警告，可以自己手动加上 `@discardableResult`，如：

```swift
@discardableResult func doSomething() -> Bool {
  return true
}
```

这样一来一切又恢复正常了。

参考: http://useyourloaf.com/blog/swift-3-warning-of-unused-result/