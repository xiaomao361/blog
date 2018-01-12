---
title: Mac解决LC_CTYPE警告的问题
tags: 
  - terminal
  - mac
date: 2018-01-12 10:55:34
categories: Tips
---

>因为这次Mac系统升级，中文翻译实在是太揪心，把系统语言更换成英文以后，iterm ssh到服务器上去总是提示`-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory`的警告，虽然不影响使用，但是很烦

<!-- more -->

**解决办法:**

Iterm2
---

打开Iterm2 > Preferences > Profiles > Terminal， 把Set locale variables automatically这个选项勾选去掉就可以了，如下图：

![iterm2](/images/iterm2.jpg)

Terminal
---

同理，Mac自带的终端（Terminal）也是使用这种办法解决，如下图

![Terminal](/images/terminal.png)

配置完成以后重启所有终端，问题解决。
