---
title: docker logs 不能查看容器日志的问题
tags: 
  - docker
date: 2018-03-07 16:14:24
categories: Tips
---

>起因：近期帮同事测试rpcx项目，run出来的container明明日志打印到终端上面，但是通过docker logs就是查看不到，试了很多办法也没能解决。

查看了下docker官方关于log的说明：[logs](https://docs.docker.com/engine/reference/commandline/logs/#extended-description)

> The docker logs --follow command will continue streaming the new output from the container’s STDOUT and STDERR.

证明常规的理解是正确的，docker logs 会查看stdout和stderr的内容以及打印到终端的内容，经过测试也确实是这样的。

<!-- more -->

但是服务器端仍然不行，在一个偶然的情况下，突然想起来服务器安装的docker比较早，当时应该是1.12版本的，去查看了下，果然是。

把同样的代码拿到本机上测试，发现是能查看日志的，那么问题就应该是版本了。

开始按照[官网的教程](https://docs.docker.com/install/linux/docker-ce/centos/)(我们的是centos的，根据自己的系统选择安装教程)给docker升级版，安装最新的 docker-ce 17版本。

试一下，果然可以了，至此问题解决，但是到底为什么1.12不行还在研究中，不过优先打算把现有服务器的docker版本统一进行下升级

*ps：从1.12 升级到ce以后，仿佛目录结构调整了，所有的容器不能启动，同时镜像可用，但是不能删除（这里面还涉及docker镜像存储目录的问题。）建议先把镜像导出，然后编写好容器运行的脚本，升级完成后再统一run出来新的容器。*
