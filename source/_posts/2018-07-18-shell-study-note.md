---
title:  Shell 的一些笔记
date: 2018-07-18 04:06:52
tags: [shell] 
---


### if -e and -n的意思？

如果目标不为 null 则返回 true
-e returns true if the target exists. Doesn't matter if it's a file, pipe, special device, whatever. The only condition where something may exist, and -e will return false is in the case of a broken symlink.

For example:

```ruby
$ ln -s foo bar
	
$ [ -e foo ]; echo $?
1
	
$ touch bar
	
$ [ -e foo ]; echo $?
0
```

<!-- more -->

In bash you can do help test to see what test options you have.

[ is usually part of your shell. In bash the options and behaviors are defined by bash. It is also kind of a synonym for test. In bash you can do help test to see all the options it supports.
The only real difference between [ and test should be that [ requires a ] after your arguments, whereas test does not. They otherwise work the same, [ -e foo ] is equivalent to test -e foo.

There is also /usr/bin/[ for shells which do not have [ built in. There is no man page for this though. But there is also a /usr/bin/test, and my system does have a man test which covers the options. I haven't tested, but I'd bet all the options supported by /usr/bin/test work on /usr/bin/[.

 

Primary |	Meaning
---- | -----
[ FILE1 -ef FILE2 ]	| True if FILE1 and FILE2 refer to the same device and inode numbers.
[ -o OPTIONNAME ]	| True if shell option "OPTIONNAME" is enabled.
[ -z STRING ] |	True of the length if "STRING" is zero.
[ -n STRING ] or [ STRING ]	| True if the length of "STRING" is non-zero.


More details: [Introduction to if](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html)

### Tests

* 比较操作符

整型比较符：

| 操作符 | 说明 | 例子 |
| ---- | ---- | ---- |
| -eq | 相等 | if [ "$a" -eq "$b" ] |
| -ne | 不相等 |
| -gt | 大于 |
| -ge | 大于或等于 |
| -lt | 小于 |
| -le | 小于或等于 |
|  <  | 小于 | (("$a" < "$b")) |
|  <= | 小于或等于 | |
|  >  | 小于 |  |
|  >= | 大于或等于 | |
 
字符串比较符：

| 操作符 | 说明 | 例子 |
| ---- | ---- | ---- |
|  =  | 等同 | |
| ==  | 相当于= | |
| !=  | 不相同 | |
| <   | | if [[ "$a" < "$b" ]]<br /> if [ "$a" \\< "$b" ] |
| -z  | 字符串为 null | if [ -z "$s" ] |
| -n  | 字符串不为 null | 


### How to exit if a command failed?

[http://stackoverflow.com/questions/3822621/how-to-exit-if-a-command-failed](http://stackoverflow.com/questions/3822621/how-to-exit-if-a-command-failed)

If you want that behavior for all commands in your script, just add

```ruby
 set -e 
 set -o pipefail
```
at the beginning of the script. This pair of options tell the bash interpreter to exit whenever a command returns with a non-zero exit code.

This does not allow you to print an exit message, though.

### Quoting Variables

双引号"" 的作用是使里面的字符除了$`\之外都不作解析，因此在使用变量$variable 的时候尽量加上双引号，以减少符号的解析和防止误解析。

单引号'' 则不会解析里面的$字符。

### Exit and Exit Status

每个命令都会返回退出状态，成功的命令返回状态0，而失败的命令则返回非零。

$?获取前面最后一条命令的退出状态。

### Grep

Convert binary file to string and search string.

    strings <file_path> | grep "<string>"

Find more files to search the string.

    find . -name "*.zip" | xargs string | grep "string"

*The End*
