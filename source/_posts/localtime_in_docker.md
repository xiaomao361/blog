---
title: 修改docker时区及jvm时区
tags: docker
date: 2018-01-03 10:54:34
categories: Linux
---


>起因是使用的项目全部容器化以后，发现java的日志时间不对，通过修改过localtime发现，系统时间改变了，但是java应用的log里面时间还是不对。

**原因是因为jvm调用的是timezone，修改localtime只能改变系统时间**

如果是手动操作可以通过命令`dpkg-reconfigure tzdata`重新选择下时区，就可以了。不过每次都要去修改实在是太麻烦了，而且经常忘记。所以打算看看能不想修改下根系统镜像。

通过修改[java8](https://hub.docker.com/_/java/)的官方镜像的[Dockerfile](https://github.com/docker-library/openjdk/blob/master/8-jdk/Dockerfile)来实现
<!-- more -->

添加如下配置

```
# change local timezone
RUN rm -rf /etc/localtime
RUN ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ENV TZ="Asia/Shanghai"
```
重新build下就好了。

这样可以获得一个时区是shanghai的镜像，在通过这个基础镜像，去build其他的镜像比如maven什么的，jvm的时间就正常了。

>ps：其实可以考虑，在国外的vps上build，然后push到docker hub，在从国内pull，整体会快很多。
