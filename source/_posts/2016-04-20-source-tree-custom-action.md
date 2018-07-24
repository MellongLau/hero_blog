---
title: 强大的Git客户端：SourceTree插件开发指南
date: 2016-04-20 20:35:37
tags: [git, sourcetree, shell]
---


## SourceTree是什么？
简单来说SourceTree是一款免费git图形化操作软件，功能很全，使用起来非常方便，相信不少开发者已经在使用这款软件。

具体还是来看看网上的介绍：

> SourceTree是Windows 和Mac OS X 下免费的Git 和Hg 客户端，拥有可视化界面，容易上手操作。 同时它也是Mercurial和Subversion版本控制系统工具。 支持创建、提交、clone、push、pull 和merge等操作。

<!-- more -->

简单说一下我的感受，一开始使用git的时候，基本上都是直接敲命令的，（没有好的软件，只能自我安慰使用命令行对学习git更有利，苦逼的程序猿~），也不知道有什么GUI软件比较好用，直到后来知道了SourceTree，使用上一段时间就彻底离不开它了，功能强大，界面漂亮，用起来顺手，跨平台，还持续更新，最重要的是免费，你没看错，是免费（重要事情说两遍~就可以了），有中文版本（虽然我不喜欢用中文版，目前还是用的英文版，原因是中文版看不出来对应的git命令是什么，个人建议大家也用英文版）。

*郑重声明一下，这篇文章不是软文。（如果SourceTree的作者看到这篇文章觉得不错的话…，可以和我联系，我这里可以接收美金，怎么联系到我？可以点击查看我的个人信息，微信，主页，邮箱都可以，好吧，我承认我想多了）。*

今天所说的插件开发，实际上是SourceTree一个叫Custom Action的功能，SourceTree从v1.3开始就增加了这个功能，这个功能可以让我们可以添加自定义的扩展动作，也就是我们经常说的插件，下面就用实际例子来让大家看看在实际中可以做些什么。

## 开始动手

### 加入Open In Sublime Text 2功能

举第一个栗子，我们可以使用Sublime Text 2打开当前选中的文件。

*以下下步骤以英文版为准，中文版的请自行翻译…*

Custom Actions 页面点击Add添加一个名为 `Open In Sublime Text 2` 的动作，右边的编辑框可以添加快捷键，接着拷贝下面代码到 `Script to run` 编辑框中

> /Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl

Parameters添加 `$FILE`， 最后点击OK保存。
添加好的界面是这样子的：

![SourceTree](/blogImages/sourcetree_001.png)

至此，我们已经成功实现了这个功能。使用的时候只要选中要打开的文件，点击右键 `Custom Actions` >  `Open In Sublime Text 2` 即可。

![SourceTree](/blogImages/sourcetree_002.png)

### 加入Open Workspace和Open Xcodeproj功能
接下来，我们再来做一个稍微复杂点的栗子。

每次打开SourceTree的仓库列表或者进入仓库的时候，如果我们要打开这个仓库的项目文件，还得先去到这个项目的文件夹找到后再打开，如果有一个功能可以直接点击一个按钮就可以自动找到并打开这个仓库里面所有的xcworkspace或者xcodeproj文件就好了，值得庆幸的事，我们可以做到这样的插件，具体做法如下：  

1. 新建一个自定义动作分别填入下面内容

* 打开xcworkspace文件

|字段名|内容|
| ---- | ---- | 
|Menu Caption| Open Workspace|
|Script to run | /bin/bash|
|Parameters| `/Users/.../open_xcode_project.sh $REPO xcworkspace`|


2. 再新建建一个自定义动作分别填入下面内容

* 打开xcodeproj文件

|字段名|内容|
| ---- | ---- | 
|Menu Caption| Open Xcodeproj| 
|Script to run| /bin/bash  |
|Parameters| `/Users/.../open_xcode_project.sh $REPO xcodeproj`|

3. 新建一个名为`open_xcode_project.sh`文件，路径和上面的路径一致：`/Users/.../open_xcode_project.sh`，路径是你自己定的，不要和我一样也加`...`，内容如下：

```bash
#仓库路径
REPO_PATH=$1
#文件的类型
OPEN_TYPE=$2

#判断打开项目文件的类型，根据类型筛选出项目文件路径
if [ $OPEN_TYPE = "xcodeproj" ]; then
    LIST=`find $REPO_PATH -name "*.xcodeproj" | grep -v "Pods.xcodeproj"`
else
    LIST=`find $REPO_PATH -name "*.xcworkspace" | grep -v ".xcodeproj/project.xcworkspace"`
fi

for ITEM in $LIST
do
#打开项目文件
open $ITEM

done
```

上面用到的 `open_xcode_project.sh` 文件我已经上传到github，传送门：[SourceTree Custom Action](https://github.com/MellongLau/SourceTree-Custom-Action)

完成上面这几步后，在仓库右键就可以看到新添加的两个功能，如下图，点击对应的功能程序就会自动打开该仓库下的项目文件，不得不说太方便了！满满的成就感有没有！

![SourceTree](/blogImages/sourcetree_003.png)

不难看出，这个插件主要是通过shell脚本来完成，把仓库的路径和打开文件的类型传给脚本来进行处理，脚本过滤出目标的文件路径并依此使用默认的软件（也就是Xcode）来打开项目文件。

## 最后
这篇文章只是抛砖引玉，你可以做到更多更棒的功能，只要你对shell命令足够熟悉，当然，想法最重要，如果有好的想法欢迎你共享出来，只有分享才能相互进步。

另外，我建了一个SourceTree的Custom Action github仓库：[SourceTree Custom Action](https://github.com/MellongLau/SourceTree-Custom-Action)，希望`有志之士`（说的就是你）一起来维护，来给我pull request吧。希望看到不久的将来我的SourceTree的Custom Action菜单满满的都是各种各样的功能。

*The End.*