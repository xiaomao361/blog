---
title: Mac下安装很酷的老终端
tags: terminal
date: 2017-12-29 09:53:34
categories: Linux
copyright: false
---

**效果演示**

![cool-retro-term](/images/old.gif)

如果喜欢玩辐射的话，这是个看起来特别像哔哔小子那个风格的终端
<!-- more -->

在github上发现的一个特别有意思的项目叫做cool-retro-term，自己折腾了会，在mac上成功安装，以下是安装教程：

该项目用QML编写，所以需要用qmake来编译，如果已经安装过QT，请略过这部分。

安装前准备：
---
+ 安装Xcode，并且同意licence agreement
+ 安装brew，（brew或者macports都可以，本文使用brew）

```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
安装qt5
---
直接使用brew 安装 qt5就行，不过本次brew更新以后不再需要`/usr/local`所有者了，所以建议还是更新一下。
```bash
$ brew update
$ brew install qt5
```
设置系统变量
```
export CPPFLAGS="-I/usr/local/opt/qt5/include"
export LDFLAGS="-L/usr/local/opt/qt5/lib"
export PATH=/usr/local/opt/qt5/bin:$PATH
```
安装cool-retro-term
---
```bash
git clone --recursive https://github.com/Swordfish90/cool-retro-term.git
cd cool-retro-term
qmake && make
mkdir cool-retro-term.app/Contents/PlugIns
cp -r qmltermwidget/QMLTermWidget cool-retro-term.app/Contents/PlugIns
open cool-retro-term.app
```
其他问题
---
安装以后，使用倒是没什么问题，就是每次exit退出的时候，会有一个崩溃的报错。

> 如果遇到问题，可以留言或者发邮件一起讨论，或者参考原作者github
>（包含其他操作系统的安装教程）[cool-retro-term](https://github.com/Swordfish90/cool-retro-term)
