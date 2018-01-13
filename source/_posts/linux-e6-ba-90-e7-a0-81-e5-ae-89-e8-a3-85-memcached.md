---
title: Linux 源码安装 memcached
tags:
  - Linux &amp; Fedora
  - memcached
id: 1699
categories:
  - Linux
date: 2014-04-11 12:10:52
---

> 最近一直在源码编译，各种东西，疯了

下载
`
# cd /tmp

# wget http://cloud.github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz

# wget https://memcached.googlecode.com/files/memcached-1.4.15.tar.gz
`

安装libevent
`# tar zxvf libevent-2.0.21-stable.tar.gz
# cd libevent-2.0.21-stable
指定一个安装路径，即./configure -prefix=/usr
# ./configure -prefix=/usr
# make
# make install`

测试libevent是否安装成功<!--more-->
`
# ls -al /usr/lib | grep libevent
lrwxrwxrwx   1 root root      21 Apr 11 19:00 libevent-2.0.so.5 -> libevent-2.0.so.5.1.9
-rwxr-xr-x   1 root root  776705 Apr 11 19:00 libevent-2.0.so.5.1.9
-rw-r--r--   1 root root 1003126 Apr 11 19:00 libevent.a
-rwxr-xr-x   1 root root     935 Apr 11 19:00 libevent.la
lrwxrwxrwx   1 root root      21 Apr 11 19:00 libevent.so -> libevent-2.0.so.5.1.9
lrwxrwxrwx   1 root root      26 Apr 11 19:00 libevent_core-2.0.so.5 -> libevent_core-2.0.so.5.1.9
-rwxr-xr-x   1 root root  463243 Apr 11 19:00 libevent_core-2.0.so.5.1.9
`
安装成功

安装memcached，同时需要安装中指定libevent的安装位置
`# cd /tmp
# tar zxvf memcached-1.4.15.tar.gz
# cd memcached-1.4.15
指定libevent的安装路径即./configure –with-libevent=/usr
# ./configure -with-libevent=/usr
# make
# make install`

测试是否成功安装memcached：
`# ls -al /usr/local/bin/mem*
-rwxr-xr-x 1 root root 137986 11?? 12 17:39 /usr/local/bin/memcached`
安装成功

启动Memcached 也可以启动多个守护进程，不过端口不能重复。
`# memcached -d -m 64m -u root -l 192.168.1.1 -p 11211 -c 256 -P /tmp/memcached.pid`
> -d选项是启动一个守护进程，> 
> -m是分配给Memcache使用的内存数量，单位是MB，我这里是64MB，> 
> -u是运行Memcache的用户，我这里是root，> 
> -l是监听的服务器IP地址，如果有多个地址的话，我这里指定了服务器的IP地址192.168.1.1，> 
> -p是设置Memcache监听的端口，我这里设置了12000，最好是1024以上的端口，> 
> -c选项是最大运行的并发连接数，默认是1024，我这里设置了256，按照你服务器的负载量来设定，> 
> -P是设置保存Memcache的pid文件，我这里是保存在 /tmp/memcached.pid

结束Memcache
`# kill `cat /tmp/memcached.pid``