---
title: 复制树莓派系统Micro SD卡
tags: 
  - pi
date: 2018-04-10 14:09:34
categories: Tips
---


>起因：最近为了玩基于树莓派ARM架构的docker，测试了多个树莓派，但是每个树莓派系统都安装调试太麻烦了。

**可以使用dd命令进行SD卡的复制**

<!-- more -->
把新的SD卡插入树莓派任意usb口

在终端下使用命令 `sudo fdisk -l`查看当前磁盘情况，使用如下命令进行SD复制

```bash
dd bs=4M if=/dev/mmcblk0 of=/dev/sda
```

+ `/dev/mmcblk0`是当前系统路径 `/dev/sda`是新插入的SD卡的路径
+ 4M最好大写，有一些时候写4m会报错。
+ 同样的操作我在mac上也进行过，但是复制完了，新的卡不能使用，不知道为什么，在树莓派上操作就可以。
