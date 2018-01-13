---
title: Apache Python mod_wsgi的编译安装
tags:
  - apache
  - mod_wsgi
id: 1660
categories:
  - Linux
date: 2014-04-10 09:03:04
---

说明一下，装完apache后，再装mod_wsgi提示没有apxs,然后网上找，说是直接yum个 httpd-devel就可以了。但我的是gentoo , 源码安装的apache,然后就去找httpd-devel的源码包，怎么也没找着。原来源码安装时就包含了apxs，在/usr/local/apache2/bin/apxs。这样就不用./configure --enable-shared 配置而是直接定义好就可以了。
`# wget https://modwsgi.googlecode.com/files/
#./configure --with-apxs=/usr/local/apache2/bin/apxs --with-python=/usr/bin/python
# make && make install
# chmod 755 /usr/local/apache2/modules.d/mod_wsgi.so //不同Linux可能不同`

修改httpd.conf (gentoo 在这里 /etc/apache2/httpd.conf) 配置,增加
LoadModule wsgi_module modules/mod_wsgi.so