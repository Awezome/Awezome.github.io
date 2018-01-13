---
title: Gentoo apache2 启动失败解决办法
tags:
  - apache
  - gentoo
id: 1656
categories:
  - Linux
date: 2014-04-09 22:39:44
---

gentoo 装个软件真麻烦。/etc/init.d/apache2 start 结果出现：
`* Starting apache2 ...
apache2: apr_sockaddr_info_get() failed for gentoo
apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.0.1 for ServerName
* start-stop-daemon: failed to start `/usr/sbin/apache2'
`

解决：
`命令 hostname 查看当前主机名，比如为 ggeenn ;
cat /etc/hosts
127.0.0.1       localhost
::1             localhost`
修改为
`127.0.0.1       ggeenn localhost
::1             ggeenn localhost`