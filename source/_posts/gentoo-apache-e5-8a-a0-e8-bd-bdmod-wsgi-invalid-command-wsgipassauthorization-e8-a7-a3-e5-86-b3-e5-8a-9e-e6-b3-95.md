---
title: Gentoo Apache 加载mod_wsgi Invalid command 'WSGIPassAuthorization' 解决办法
tags:
  - apache
  - gentoo
  - mod_wsgi
id: 1658
categories:
  - Linux
date: 2014-04-10 08:57:32
---

Gentoo Apache 加载mod_wsgi 启动apache 出现  Invalid command 'WSGIPassAuthorization' ，就是mod_wsgi没 load 上，于是在 /etc/apache2/apache2.conf 加上 
`LoadModule wsgi_module modules/mod_wsgi.so`
还不行，版本问题？于是重新编译了一个老版本的，还是不行，后来发现 文件改错了，应该在
`/etc/apache2/httpd.conf` 加上。重启ok