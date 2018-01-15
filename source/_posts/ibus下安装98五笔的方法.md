---
title: ibus下安装98五笔的方法
tags:
  - Linux
id: 408
categories:
  - Linux
date: 2011-11-16 15:23:19
---

1. 下载[ibus-table-wubi](http://ibus.googlecode.com/files/ibus-table-wubi-1.2.0.20091227.tar.gz "ibus-table-wubi-1.2.0.20091227.tar.gz")进行解压后，到table文件夹中找到wubi98.txt;

2. 在终端运行cd 切换到wubi98.txt所在的文件夹
3. 运行ibus-table-createdb -s wubi98.txt编译wubi98.db文件
4. 运行sudo cp wubi98.db /usr/share/ibus-table/tables 把wubi98.db复制到系统位置
5. 运行cd 切换到解压后的icons文件夹
6. 运行sudo cp wubi98.svg /usr/share/ibus-table/icons 复制图标
7. 重启ibus首选项输入选择输入法汉语五笔98添加，再重启ibus。

后又参照“将海峰五笔码表转到ibus下使用”（http://blog.linjian.org/articles/sunwb-ibus/）把海峰五笔98也安装成功。
 下面网上安装方法

1. 下载 wget http://ibus.googlecode.com/files/ibus-t ... 219.tar.gz 
2. tar -zxvf ibus-table-wubi-1.1.0.20090219.tar.gz 
3. cd ibus-table-wubi-1.1.0.20090219 
4. ./configure --prefix=/usr --enable-wubi98 --disable-wubi86 
5. sudo make install 
6. 系统 － 首选项 － iBus首选项中添98五笔