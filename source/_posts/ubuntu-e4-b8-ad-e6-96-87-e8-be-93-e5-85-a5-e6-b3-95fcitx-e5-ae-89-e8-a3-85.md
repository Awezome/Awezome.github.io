---
title: ubuntu中文输入法Fcitx安装
tags:
  - Linux
id: 504
categories:
  - Windows
date: 2012-01-06 20:32:18
---

在ubuntu10.10系统安装成功后,系统默认的中文输入法是ibus,但是这个输入法给我们这些在windows下使用sougou用惯了的人是很不习惯的.特别是打词组的时侯,不能联想,词库不是很多.后来有了解到还有一款中文输入法Fcitx,所以换成这个之后,发现比较适合我使用,虽然存在很多不足.

首先来认识下Fcitx,它是Free Chinese Input Toy for X的简称,中文名叫小企鹅输入法,包括拼音和双拼输入法.目前最新版本为4.0.1,相比之前的版本,新增很多功能包括新的皮肤、词库和详细设置界面。

 之前这款输入法是个人所做(不过已经停止开发)，目前官方主页已经转移到google code上: http://code.google.com/p/fcitx/
上面有最新的版本下载和皮肤，词库。<!--more-->
 安装方法如下：
 sudo add-apt-repository ppa:wengxt/fcitx-nightly   增加ppa源
 sudo apt-get update   更新软件库
 sudo apt-get install fcitx fcitx-config-gtk fcitx-sunpinyin  安装fcitx和fcitx-sunpinyin输入法
 sudo apt-get install fcitx-table-all   安装码表
 到目前为止输入法已经安装完成了.
 接下来输入:im-switch -s fcitx -z default 把fcitx设置为默认输入法
 或者通过:系统—系统管理—语言支持,打开语言和文本设置项,在语言分页中的键盘输入方式系统中选择fcitx.
 另外,可以右击输入法进行配置,包括皮肤,快捷键等.