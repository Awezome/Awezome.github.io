---
title: rsync 用法总结
tags:
  - rsync
id: 1515
categories:
  - Linux
date: 2014-03-25 15:08:21
---

语法：
`rsync options source destination
-z is to enable compression
-v verbose
-r indicates recursive
-W transfer the whole files
-a	Recursive mode
	Preserves symbolic links
	Preserves permissions
	Preserves timestamp
	Preserves owner and group
--progress show progress
--delete delete the unuseful files
--include
--exclude
--max-size='100K'  no large files`
<!--more-->
1.同步两个本地目录
`rsync -zvr /var/opt/installation/inventory/ /root/temp`
2.同步一个文件
`rsync -v /var/lib/rpm/Pubkeys /root/temp/`
3.从本地同步到服务器
`rsync -avz /root/temp/ thegeekstuff@192.168.200.10:/home/thegeekstuff/temp/`
4.从服务器同步到本地
`rsync -avz thegeekstuff@192.168.200.10:/var/lib/rpm /root/temp`
5.仅同步目录结构
`rsync -v -d thegeekstuff@192.168.200.10:/var/lib/ .`
6.查看改变的文件
`rsync -avzi thegeekstuff@192.168.200.10:/var/lib/rpm/ /root/temp/`

实例
`rsync -r --del --exclude="abc/config" --exclude=".idea/*"  rsync://192.168.1.5/all /opt/web/htdocs/`

[http://www.thegeekstuff.com/2010/09/rsync-command-examples/]