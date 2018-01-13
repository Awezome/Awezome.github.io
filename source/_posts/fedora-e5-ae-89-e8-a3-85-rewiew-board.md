---
title: Fedora 安装 Rewiew Board
tags:
  - fedora
  - Linux &amp; Fedora
  - reviewboard
id: 1620
categories:
  - Linux
date: 2014-04-07 11:50:14
---

安装 Rewiew Board太复杂，依赖很多，嫌麻烦有个一键安装：https://bitnami.com/stack/reviewboard。下面是手动安装过程
**安装**
`$ yum install httpd mod_wsgi
$ yum install mysql mysql-server mysql-devel
$ yum install memcached
$ yum install patch
$ yum install subversion pysvn
$ yum install rpm-build gcc-c++ gcc
$ yum install python-setuptools
$ yum install python-devel
$ easy_install -U setuptools
$ easy_install python-memcached
$ easy_install mysql-python
$ easy_install ReviewBoard`<!--more-->
极有可能还要装
`easy_install markdown
easy_install docutils
easy_install "django-pipeline<1.3"
easy_install "Djblets<0.8"
easy_install "django-evolution<0.7"
easy_install "Django<1.5"
easy_install ecdsa
`
**创建**
`memcached -d -m 64m -p 11211 //不要sudo
service httpd start
systemctl start mysqld.service
`
数据库设置`
设置编码：http://www.awezome.net/1640/
创建数据库 reviewboard
mysql> create database reviewboard default charset utf8 collate utf8_general_ci;
`
**安装**
`rb-site install /var/www/reviewboard`
`
Domain = localhost
Root Path = /
simple media url=static/
upload Media URL = media/
Database Type = mysql
Database Name = reviewboard
Database server = localhost
Database username = ''
Database password = ''
Cache Type = memcache
Memcache Server = memcached://localhost:11211/
Webserver = apache
Python loader = wsgi`
**配置**
`$ chown -R 777 /var/www/reviewboard/htdocs/media/uploaded
$ chown -R 777 /var/www/reviewboard/data
cp /var/www/reviewboard/conf/apache-wsgi.conf /etc/httpd/conf.d/reviewboard.conf`
**问题**
`Setup script exited with error: command 'gcc' failed with exit status 1
$ yum install rpm-build gcc-c++ gcc`

`error: Could not find suitable distribution for Requirement.parse('mysql-python')
再试一次`

`pkg_resources.DistributionNotFound: Djblets
$ easy_install --upgrade Djblets`

`Review Board is taking a nap
修改/etc/selinux/config 文件,将SELINUX=enforcing改为SELINUX=disabled .重启`

参考
http://www.reviewboard.org/
http://www.reviewboard.org/docs/manual/1.7/admin/installation/linux/#installing-python-setuptools
http://www.reviewboard.org/docs/manual/dev/admin/sites/rb-site/
http://blog.csdn.net/shencaifeixia1/article/category/1280711