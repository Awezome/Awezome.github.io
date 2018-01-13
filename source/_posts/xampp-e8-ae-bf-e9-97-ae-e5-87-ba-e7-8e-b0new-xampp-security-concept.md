---
title: xampp 访问出现New XAMPP security concept
tags:
  - phpmyadmin
id: 298
categories:
  - PHP
date: 2011-09-21 16:21:53
---

> New XAMPP security concept:
> Access to the requested directory is only available from the local network.
> This setting can be configured in the file “httpd-xampp.conf”.
解决办法：

打开httpd-xampp.conf(/xampp/apache/conf/extra/httpd-xampp.conf)

将Deny from all这一行注释掉，即
> #Deny from all
需要重启apache