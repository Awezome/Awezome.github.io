---
title: MySQL 默认字符集设置
tags:
  - MySQL
id: 1640
categories:
  - MySQL
date: 2014-04-07 18:52:55
---

在 Fedora 安装完 MySQL 后编字符编码可能是非 utf8的，这样以来像 Review Board 没设置默认的就会按非 utf8 默认字符集来安装，就不能很好的显示中文，所以安装完 MySQL 后要进行相应的编码设置。

命令行模式进入MySQL，敲入status命令:
```
Server characterset:	latin1
Db     characterset:	latin1
Client characterset:	utf8
Conn.  characterset:	utf8
```

有两个latin1 ，我们要把它干掉：
停止
MySQL Server
打开
/etc/my.cnf.d/ 有的是（/etc/mysql/my.cnf）
设置如下：
```ini
[client]
default-character-set = utf8
[mysqld]
character_set_server = utf8 
init_connect='SET NAMES utf8'
```
保存后退出
如果启动失败，用 character_set_server = utf8 这个试试

重启mysql再status
```
Server characterset:	utf8
Db     characterset:	utf8
Client characterset:	utf8
Conn.  characterset:	utf8
```
