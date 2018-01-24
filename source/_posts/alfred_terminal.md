---
title: alfred3 更换 Terminal为Iterm2
tags: 
  - terminal
  - alfred
date: 2018-01-24 14:43:24
categories: Tips
copyright: false
---

>alfred一直是mac上的效率神器，有多好用就不多说了，默认安装完成，terminal调用的是系统默认的终端，这很尴尬，查找了下，可通过以下途径换成iterm2

如图所示，把终端选项切换成`custom`
![alfred_custom](/images/alfred_custom.jpg)

<!-- more -->

把里面的内容替换成如下代码：
```
on alfred_script(q)
	if application "iTerm2" is running or application "iTerm" is running then
		run script "
			on run {q}
				tell application \":Applications:iTerm.app\"
					activate
					try
						select first window
						set onlywindow to false
					on error
						create window with default profile
						select first window
						set onlywindow to true
					end try
					tell the first window
						if onlywindow is false then
							create tab with default profile
						end if
						tell current session to write text q
					end tell
				end tell
			end run
		" with parameters {q}
	else
		run script "
			on run {q}
				tell application \":Applications:iTerm.app\"
					activate
					try
						select first window
					on error
						create window with default profile
						select first window
					end try
					tell the first window
						tell current session to write text q
					end tell
				end tell
			end run
		" with parameters {q}
	end if
end alfred_script
```

再次使用alfred调用命令或者终端，就都是iterm2了。

> 参考[custom-iterm-applescripts-for-alfred](https://github.com/stuartcryan/custom-iterm-applescripts-for-alfred)
