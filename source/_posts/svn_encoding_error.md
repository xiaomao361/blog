---
title: svn配置post-commit以后报错
tags: 
  - svn
  - linux
date: 2018-02-22 15:49:34
categories: Tips
---


>svn配置post-commit以后报错，报错svn: Can't convert string from 'UTF-8' to native encoding

**原因是因为文件语言编码和系统冲突导致的错误**

<!-- more -->

在svn目录下hooks/post-commit添加:

```bash
export LANG=zh_CN.UTF-8
```

问题就解决了。