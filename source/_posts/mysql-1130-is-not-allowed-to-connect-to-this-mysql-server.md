---
title: 'Mysql: 1130 is not allowed to connect to this MySQL server'
tags:
  - MySQL
id: 1677
categories:
  - MySQL
date: 2014-04-10 11:55:21
---

如果你想连接你的mysql的时候发生这个错误：

ERROR 1130: Host '192.168.1.3' is not allowed to connect to this MySQL server

解决方法：
如果你想允许用户myuser从ip为192.168.1.3的主机连接到mysql服务器，并使用mypassword作为密码
`GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.1.3' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'10.10.40.54' IDENTIFIED BY '123456' WITH GRANT OPTION;`