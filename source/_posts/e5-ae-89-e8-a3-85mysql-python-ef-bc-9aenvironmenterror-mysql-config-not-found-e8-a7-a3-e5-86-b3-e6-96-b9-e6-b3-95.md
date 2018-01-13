---
title: '安装MySQL-python：EnvironmentError: mysql_config not found 解决方法及源码安装  '
tags:
  - MySQL
  - Python
id: 1663
categories:
  - MySQL
date: 2014-04-10 09:08:26
---

这个问题通常有两种，我遇到的是第二种
1.依赖
` apt-get install libmysqld-dev
 yum install python-devel
 yum install mysql-devel`
2.配置文件有问题
执行  find / -name mysql_config 在/usr/bin/下找到了这个文件
修改MySQL-python-1.2.3目录下的site.cfg文件
去掉mysql_config=XXX这行的注释，并改成mysql_config=/usr/bin/mysql_config （就是刚才find找到的路径）
再执行
`python setup.py build
python setup.py install`
进行安装。
也可以用
直接在压缩包里进行修改site.cfg
再
`easy_install ./MySQL-python-1.2.3.zip`