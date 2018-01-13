---
title: Linux 安装 xampp及常见问题总结
tags:
  - apache
  - Linux
  - MySQL
  - php
id: 502
categories:
  - Linux
date: 2012-01-06 20:29:29
---

安装完要做的事情
`ln -sf /opt/lampp/xampp /usr/bin
ln -sf /opt/lampp/bin/mysql /usr/bin
ln -sf /opt/lampp/bin/php /usr/bin
xampp security `

常用命令
`xampp start
xampp stop
xampp restart
xampp startapache
xampp stopapache
xampp startmysql
xampp stopmysql
xampp startftp
xampp stopftp`<!--more-->

XAMPP 重要的文件和目录
`XAMPP 命令库
/opt/lampp/bin/`

Apache 文档根目录
/opt/lampp/htdocs/

Apache 配制文件
/opt/lampp/etc/httpd.conf

MySQL 配制文件
/opt/lampp/etc/my.cnf

PHP 配制文件
/opt/lampp/etc/php.ini

ProFTPD 配制文件
/opt/lampp/etc/proftpd.conf

phpMyAdmin 配制文件
/opt/lampp/phpmyadmin/config.inc.php

随系统自动启动
ln -s /opt/lampp/xampp /etc/rc.d/rc3.d/S99lampp
ln -s /opt/lampp/xampp /etc/rc.d/rc4.d/S99lampp
ln -s /opt/lampp/xampp /etc/rc.d/rc5.d/S99lampp

取消随系统自动运行
ln -s /opt/lampp/lampp K01lampp

问题总结：

如果想要普通用户能写htdocs目录，清修改目录权限。
chmod －R a＋rw ／opt／lampp／htdocs

XAMPP: Couldn’t start MySQL!解决方案 (启动不了mysql服务）
chmod 777 -R /opt/lampp/var

卸载 XAMPP，只需输入如下命令：sudo rm -rf /opt/lampp

使用配置文件中定义的控制用户连接失败 解决方法
1、在安装phpMyAdmin的根目录下找到config.inc.php配置文件（也有可能是config.sample.inc.php，先将其重命名为config.inc.php）并用记事本打开。

2、在打开的配置文件里找到$cfg['Servers'][$i]['controlpass'] = ‘*******’;”这一段其中*******就是你的密码，默认为空，将它修改成你在phpMyAdmin上修改后的密码。

3、如果你的用户名也修改过的话就找到$cfg['Servers'][$i]['controluser'] = ‘root’;这一段，其中root就是你的用户名，将它修改成你修改后的用户名。

更改htdocs目录
然后sudo cp -R /opt/lampp/htdocs /home/htdocs把整个htdocs目录复制一份放在/home下,
然后sudo chmod -R 777 /home/htdocs
最后sudo vim /opt/lampp/etc/httpd.conf修改Apache 配制文件，查找里面的/opt/lampp/htdocs全部替换改为我们刚才的htdocs目录地址/home/htdocs保存退出就可。