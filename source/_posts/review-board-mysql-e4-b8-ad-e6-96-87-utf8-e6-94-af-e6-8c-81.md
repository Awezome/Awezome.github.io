---
title: Review Board MySQL 中文 utf8 支持
tags:
  - MySQL
  - reviewboard
id: 1638
categories:
  - MySQL
date: 2014-04-07 18:36:04
---

默认 Review Board 的安装数据库编码可能会是  Latin1 ，这样就不能存中文，会出现问号或乱码。如果安装完才发现这个问题，可以这样来修改成utf8.

数据库编码转换
在mysql命令行下输入：alter database reviewboard default character set utf8;

数据表的编码转换
命令行下使用 mysqldump 导出数据语句，然后打开 sql 文件，替换其中的 Latin1 为 utf8 , 再重建或清空原数据库，再导入就行，注意这时如果重建的话编码要是 utf8