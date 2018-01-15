---
title: QT增加程序图标
tags:
  - Qt
id: 929
categories:
  - Qt
date: 2012-03-06 16:08:50
---

首先准备个ICO图标。例如：A.ico，网上有很多图标文件。
用记事本新建个txt
里面就写一行：
IDI_ICON1 ICON DISCARDABLE "A.ico"
保存，修改后缀为.rc，例如： myapp.rc
把它和图标A.ico一起复制到你的QT工程项目的目录。
打开你的QT工程文件.pro（例如 "myapp.pro" ），
在里面最后新添一行
RC_FILE = myapp.rc
保存，重新编译你的工程。
如果想换图标，就重换一个图标，重命名为A.ico替换原来的，重新编译就可以了。