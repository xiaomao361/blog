---
title: 域名增加SSL证书
tags: 
  - ssl
  - web 
date: 2018-01-05 14:40:23
categories: Document
---

>我们使用letsencrypt提供的脚本来创建证书，参考网址：https://letsencrypt.org/

准备文件：

+ letsencrypt.conf (配置文件)
+ letsencrypt.sh （脚本文件）
+ 域名
+ 服务器web容器为nginx

<!-- more -->

修改脚本文件
---

针对自己域名和nginx配置的目录情况，修改`letsencrypt.conf`配置文件

```
ACCOUNT_KEY="letsencrypt-account.key"
DOMAIN_KEY="demo.test.com.key"
DOMAIN_DIR="/system/nginx/html/"
DOMAINS="DNS:demo.test.com"
```
上述配置中，`DOMAIN_DIR`一般配置成为项目在nginx中的根目录.

在`letsencrypt.sh` 最后一行追加nginx重启命令，或者不加，回头自己重启也可以。

修改nginx配置
---
临时修改nginx配置文件`nginx.conf`或者你项目的配置文件。

在server中添加一条配置:

```
location /.well-known/acme-challenge/ {
	alias /system/nginx/html/.well-known/acme-challenge/;
    }
```

重启nginx

配置中地址，需要和之前在`letsencrypt.conf `配置文件中写的地址一致。

**脚本执行创建证书以后，会针对域名做一个验证，会写入一个文件在`/system/nginx/html/.well-known/acme-challenge/`路径中，并通过域名去访问。**

生成证书
---

以root权限 执行脚本 `sudo ./letsencrypt.sh letsencrypt.conf`

重启nginx

添加ssl
---
修改nginx配置：

修改server配置：

```
server {
    listen 443 ssl;
    ssl on;
    server_name demo.test.com;
    ssl_certificate     /path/demo.chained.crt;
    ssl_certificate_key /path/demo.test.com.key;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:ECDHE-RSA-DES-CBC3-SHA:ECDHE-ECDSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
    ssl_prefer_server_ciphers   on;
    }
```

并追加一个server

```
server {
    listen 80;
    server_name demo.test.com;
    return 301 https://demo.test.com$request_uri;
}
```

重启nginx

>ps:该证书效期3个月，到期以后需要重新执行第三步生成新的证书，并**重启nginx**，建议把重启命令写入`letsencrypt.sh`脚本最后
>并追加crontab脚本，定时生成新的证书
