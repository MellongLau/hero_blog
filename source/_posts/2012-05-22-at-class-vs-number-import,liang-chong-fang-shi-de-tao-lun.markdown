---
layout: post
title: "@class vs. #import，两种方式的讨论"
date: 2012-05-22 10:28:11 +0800
comments: true
categories: [iOS]
---
很多刚开始学习iOS开发的同学可能在看别人的代码的时候会发现有部分`#import`操作写在m文件中，而h文件仅仅使用`@class`进行声明，不禁纳闷起来，为什么不直接把`#import`放到h文件中呢？

这是因为h文件在修改后，所有import该h文件的所有文件必须重新build，因此，如果把`#import`写在h文件中，import该h文件的文件也就会产生不必要的编译，增加编译时间，特别是在项目文件多的情况下。想象一下，如果只是修改一个h文件而导致上百个文件不必要的编译，那是一件多么让人纠结的事情。。。

对于`@class`只是告诉编译器有这个class，请不要报错或警告，因此不会给编译造成影响。

什么时候用`@class`这种方式声明比`#import`好呢？

`stackoverflow`上的高手们给了不少建议：

**Randy Marsh：**

> When I develop, I have only three things in mind that never cause me any problems.

> Import super classes  
Import parent classes (when you have children and parents)  
Import classes outside your project (like in frameworks and libraries)  
For all other classes (subclasses and child classes in my project self), I declare them via forward-class.


**Justin：**

> Simple answer: You #import or #include when there is a physical dependency. Otherwise, you use forward declarations (@class MONClass,struct MONStruct, @protocol MONProtocol).

> Here are some common examples of physical dependence:

> Any C or C++ value (a pointer or reference is not a physical dependency). If you have aCGPoint as an ivar or property, the compiler will need to see the declaration ofCGPoint.  
Your superclass.  
A method you use.

最后，我建议还是养成良好的import习惯，不要偷懒都把import放在h文件中，无论参与的项目大小，养成良好的编程习惯非常重要。