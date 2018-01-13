---
title: ubuntu 11.10 安装google earth 字体不清解决办法
tags:
  - Linux
id: 554
categories:
  - Linux
  - Windows
date: 2012-01-25 21:38:07
---

本文链接地址: http://canglangxuan.com/?p=1597

ubuntu早就想安装谷歌地球了，之前的还是.bin文件，现在都是deb文件了，不错！可是会出现中文乱码问题，之前找了很久，没找到比较好的解决办法，不过下面的方法倒是很给力，Jason在ubuntu11.04下测试通过！
先到谷歌地球的官方网站下载：http://www.google.com/intl/zh-CN/earth/download/ge/
下载deb包进行安装，安装完打开Google earth后你会发现中文无法显示出来，这时候需要做一些处理：
替换文件,在终端运行下面代码：
cd /opt/google/earth/free/
sudo rm libQtCore.so.4 libQtGui.so.4 libQtNetwork.so.4 libQtWebKit.so.4
sudo apt-get install libqt4-core libqt4-gui libqt4-network libqt4-webkit libfreeimage3
修改文件：
sudo gedit /opt/google/earth/free/googleearth
在（最后一行）：
LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH ./googleearth-bin “$@”
前面加上：
export LD_PRELOAD=/usr/lib/libfreeimage.so.3
然后保存。
接着从新打开谷歌地球就可以了。