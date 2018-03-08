---
title: docker ce 配置镜像存储路径
tags: 
  - docker
date: 2018-03-08 16:21:04
categories: Tips
---

>起因：原来使用的docker版本是1.2，升级ce 17 以后发现找不到docker的配置文件了，尴尬。

我们使用的是centos系统，原来大家的做法都是修改docker的配置文件
```bash
vim /etc/sysconfig/docker

然后添加指定的参数 --graph=/var/lib/docker

```
并重启docker就可以了。

这次升级以后发现配置不在不在该位置了。

<!-- more -->

当时情况比较着急，所以采用了另一种比较取巧的办法。

由于是新安装的docker，也没有什么数据需要导入，我们挂载了一个盘，直接mount到/docker目录，这样的话，因为docker默认路径是/var/lib/docker

所以我们只需要建立一个链接

`ln -s /docker /var/lib/docker`

这样再启动docker就可以了，然后在慢慢研究该版本配置文件的问题。

如果有数据需要导出，记得提前备份。尤其是这种跨大版本升级，建议把镜像导出一份，因为目录结构不不同了，就算把整个文件复制过去，容器应该也是启动不了。