---
title: apache禁止显示目录索引
id: 1090
categories:
  - PHP
date: 2012-10-19 17:04:04
tags:
---

pache禁止显示目录索引

apache显示目录索引很不安全，下面是操作方法。

在httpd.conf文件搜索关键字"Indexes "。
<directory "/var/www/html">
    Options Indexes FollowSymLinks
    AllowOverride None
    Order allow,deny
    Allow from all
</directory>

出掉Indexes关键字，修改如下：
<directory "/var/www/html">
    Options  FollowSymLinks MultiViews
    AllowOverride None
    Order allow,deny
    Allow from all
</directory>