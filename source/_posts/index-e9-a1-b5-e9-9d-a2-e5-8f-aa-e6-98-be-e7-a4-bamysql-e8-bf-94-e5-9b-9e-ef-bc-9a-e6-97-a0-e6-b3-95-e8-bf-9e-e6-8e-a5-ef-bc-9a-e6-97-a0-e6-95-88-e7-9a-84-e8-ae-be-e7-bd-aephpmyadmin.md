---
title: 'phpMyAdmin显示“MySQL返回:无法连接:无效的设置''解决方法'
tags:
  - phpmyadmin
id: 265
categories:
  - PHP
date: 2011-09-03 08:18:28
---

phpMyAdmin的index页面只显示“MySQL返回：无法连接：无效的设置。”解决方法：
找到phpMyAdmin的配置文件config.inc.php
将其中的
> $cfg['Servers'][$i]['auth_type'] = 'config';
改为：
> $cfg['Servers'][$i]['auth_type'] = 'cookie';
就OK了~