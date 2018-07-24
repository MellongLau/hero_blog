---
title: iOS 物理引擎实践之模拟粒子运动
date: 2018-04-29 10:13:50
tags: [iOS, Swift] 
---

最近看到一个使用 javascript 编写的模拟粒子运动的库[verlet-js](https://github.com/subprotocol/verlet-js)，效果很不错，于是就想把他移植到 iOS 上，花了点时间使用 swift 把代码移植了过来，最后还加了一个粒子系统的 demo，源代码已经放到 github 上了: 

[https://github.com/MellongLau/ParticleAnimation](https://github.com/MellongLau/ParticleAnimation)。

下面放上Demo效果截图：

![Shape](https://raw.githubusercontent.com/MellongLau/ParticleAnimation/master/screenshot-shape.gif)

<!-- more -->

![Tree](https://raw.githubusercontent.com/MellongLau/ParticleAnimation/master/screenshot-tree.gif)

![Cloth](https://raw.githubusercontent.com/MellongLau/ParticleAnimation/master/screenshot-cloth.gif)

![Spider](https://raw.githubusercontent.com/MellongLau/ParticleAnimation/master/screenshot-spider.gif)

![Particle System](https://raw.githubusercontent.com/MellongLau/ParticleAnimation/master/screenshort-particle-system.gif)

*The End*
