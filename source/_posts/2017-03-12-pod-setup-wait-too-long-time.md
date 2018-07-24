---
title: pod setup命令失败解决方法
date: 2017-03-12 08:00:56
tags: cocoapods
---

最近运行pod setup出现以下问题：

```ruby
remote: Compressing objects: 100% (34/34), done.
error: RPC failed; curl 56 SSLRead() return error -3613.00 KiB/s
fatal: The remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed
```

<!-- more -->

我们知道 cocoapods 的 sepcs 文件是放在这个目录里面

	~/.cocoapods/repos

所以可以直接 cd 到该目录下然后运行命令: 

	git clone https://github.com/CocoaPods/Specs.git master

```ruby
Cloning into 'master'...
remote: Counting objects: 894306, done.
remote: Compressing objects: 100% (56/56), done.
^Cceiving objects:   6% (53659/894306), 10.39 MiB | 216.00 KiB/s
...
```

然后会发现clone 的文件很大，由于速度也很慢，一不小心就失败了。

其实我们无需全部 clone 下来，可以只 clone 最近一个 commit 的全部代码就可以了。

	git clone --depth=1  https://github.com/CocoaPods/Specs.git master

```ruby
Cloning into 'master'...
remote: Counting objects: 261047, done.
remote: Compressing objects: 100% (179891/179891), done.
remote: Total 261047 (delta 44498), reused 253721 (delta 44409), pack-reused 0
Receiving objects: 100% (261047/261047), 44.76 MiB | 124.00 KiB/s, done.
Resolving deltas: 100% (44498/44498), done.
Checking connectivity... done.
Checking out files: 100% (118515/118515), done.
```

不用多久就 clone 成功了，这时候就直接可以使用pod install 最新版本的 library 了。
