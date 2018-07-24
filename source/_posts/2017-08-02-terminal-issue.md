---
title: 终端使用中遇到的问题
date: 2017-08-02 10:13:50
tags: [linux,terminal,shell,git,svn]
---

### 文件出现 ^M 问题

`^M` 是回车符，他是源于 DOS/Windows 的，但是在 Unix 的系统中这样的符号在执行的时候会报错，在使用 vim 查看文件的时候会直接看到`^M` 而不是换行。

想替换掉`^M`，换成`\r`或者`\n`, 千万不要直接打上符号`^M` 进行替换，`^M` 这个符号要使用 `ctrl+v` & `ctrl+m` 生成，在 vim 中使用 

> :%s/^M/\r/g

进行替换所有回车符。

<!-- more -->

### Git SVN Issue
使用 git svn 的时候报一下错误：

> Can't locate SVN/Core.pm in @INC (you may need to install the SVN::Core module) (@INC contains: /usr/local/git/lib/perl5/site_perl/5.18.2/darwin-thread-multi-2level /usr/local/git/lib/perl5/site_perl/5.18.2 /usr/local/git/lib/perl5/site_perl /Library/Perl/5.18/darwin-thread-multi-2level /Library/Perl/5.18 /Network/Library/Perl/5.18/darwin-thread-multi-2level /Network/Library/Perl/5.18 /Library/Perl/Updates/5.18.2 /System/Library/Perl/5.18/darwin-thread-multi-2level /System/Library/Perl/5.18 /System/Library/Perl/Extras/5.18/darwin-thread-multi-2level /System/Library/Perl/Extras/5.18 .) at /usr/local/git/lib/perl5/site_perl/Git/SVN/Utils.pm line 6.
BEGIN failed--compilation aborted at /usr/local/git/lib/perl5/site_perl/Git/SVN/Utils.pm line 6.
Compilation failed in require at /usr/local/git/lib/perl5/site_perl/Git/SVN.pm line 25.
BEGIN failed--compilation aborted at /usr/local/git/lib/perl5/site_perl/Git/SVN.pm line 32.
Compilation failed in require at /usr/local/git/libexec/git-core/git-svn line 21.
BEGIN failed--compilation aborted at /usr/local/git/libexec/git-core/git-svn line 21.

参考这个问题:   
[http://stackoverflow.com/questions/16578465/on-osx-using-sourcetree-git-svn-getting-cant-locate-svn-core-pm-in-inc](http://stackoverflow.com/questions/16578465/on-osx-using-sourcetree-git-svn-getting-cant-locate-svn-core-pm-in-inc)

* 解决方法：

> sudo mkdir /Library/Perl/5.18/auto

> sudo ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.18/darwin-thread-multi‌-2level/SVN /Library/Perl/5.18/darwin-thread-multi-2level

> sudo ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.18/darwin-thread-multi‌-2level/auto/SVN /Library/Perl/5.18/auto/


