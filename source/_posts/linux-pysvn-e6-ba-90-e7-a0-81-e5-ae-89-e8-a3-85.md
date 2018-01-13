---
title: Linux pysvn 源码安装
tags:
  - Linux &amp; Fedora
  - pysvn
  - Python
id: 1688
categories:
  - Linux
date: 2014-04-11 11:34:38
---

> gentoo n年没更新了，装什么都麻烦

下载
http://pysvn.barrys-emacs.org
http://pysvn.barrys-emacs.org/source_kits/pysvn-1.7.8.tar.gz

解压 （可以参考解压出来的INSTALL.html）
`tar xzf pysvn-1.7.8.tar.gz
cd pysvn-1.7.8/Source
python setup.py backport
python setup.py configure
make`

安装 （不知道 python-路径 看这里 http://www.awezome.net/1690/ ）
`mkdir python-路径/site-packages/pysvn
cp pysvn/* python-路径/site-packages/pysvn`

测试
`python
import pysvn`
没报错应该就可以了