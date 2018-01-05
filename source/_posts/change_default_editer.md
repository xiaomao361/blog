---
title: 修改linux默认的编辑器
tags: 
  - linux
date: 2017-12-29 12:40:34
categories: Tips
---

>起因是在研究在终端下的一些快捷操作的时候，发现通过`ctrl+x` `ctrl+e`可以用编辑器来编写命令，然后悲剧的发现，一般unix系统下默认的编辑器是emacs，我是vim党...所以需要修改下。

修改默认的编辑器
---

方法很简单，编辑用户的配置文件
```bash
$ vim ~/.bash_prifile
```
<!-- more -->
加入下面的配置
```
export VISUAL=vim
export EDITOR="$VISUAL"
```

```bash
$ source ~/.bash_profile
```
就切换成vim了

至于为什么要使用`ctrl+x` `ctrl+e`可以用编辑器来编写命令呢
当你需要在终端编写循环或者判断的命令，就知道这样操作有多方便了。

一些快捷操作
---

| 操作           | 功能    | 
| ------------- |:-------------:| 
| tab           | 命令补全        | 
| ctrl+r        | 搜索命令历史      | 
| ctrl+w        | 删除当前命令的第一个单词      | 
| ctrl+u        | 删除到行首      | 
| ctrl+k        | 删除到行尾      | 
| ctrl+a        | 移动到行首     | 
| ctrl+e        | 移动到行尾     | 
| ctrl+x ctrl+e | 调用自己定义的编辑器来编辑当前命令行      | 
| cd -        | 回到上次的目录     | 
| pgrep      |  直接查询进程的pid（苦逼的想起了自己以前是 grep 以后再awk...）      | 
