---
title: 'bash: mysql: command not found 解决方法'
tags:
  - MySQL
id: 1654
categories:
  - MySQL
date: 2014-04-09 22:25:21
---

装完mysql, 远行 mysql -u root 发现 -bash: mysql: command not found
在/usr/bin 建个连接就可以
`# ln -s /usr/local/mysql/bin/mysql /usr/bin`