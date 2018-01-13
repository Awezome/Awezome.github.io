---
title: ubuntu升级导致显示混乱及A卡安装卸载
tags:
  - Linux
id: 515
categories:
  - Linux
date: 2012-01-08 21:30:48
---

如果由于升级内核导致显示混乱，需要先用旧内核启动，执行下面的命令删除已经安装的驱动，再进入新内核安装。我的感觉是进入(recovery mode)模式，然后选择第三行 root，比较好操作。

cd /usr/share/ati/
 sudo ./fglrx-uninstall.sh

一、下载驱动和必要的文件

1、下载驱动。你最好到 ati.amd.com 下载适合你的最新的显卡驱动。

http://ati.amd.com/support/driver.html
 https://www2.ati.com/drivers/linux/ati-driver-installer-8-11-x86.x86_64.run

2、下载 linux-headers 文件和 非开源模块，这是安装驱动所必须的。我的内核版本是 2.6.24-22，你可以根据启动时候 grub 显示的内核版本号来下载适合自己的头文件。

sudo apt-get install linux-headers-2.6.24-22 linux-headers-2.6.24-22-generic linux-restricted-modules-generic

二、删除以前的 ATI 驱动和 Mesa 驱动。也许你没有安装，但如果不删除现有的显卡驱动，可能造成安装后加载模块和驱动错误，不能正常驱动。
 如果以前手工安装过驱动需要执行下面两行，如果没有，跳过。

cd /usr/share/ati/
 sudo ./fglrx-uninstall.sh<!--more-->

sudo apt-get remove xorg-driver-fglrx xserver-xorg-video-ati xserver-xgl

三、安装驱动，不要在删除步骤前安装，不然会被卸载掉的。没有什么难的，基本上下一步就行了。

sudo sh ./ati-driver-installer-8-11-x86.x86_64.run

安装完毕后，把配置文件初始化一下，执行

sudo aticonfig –initial -f

四、生成 modules.dep 和 map 文件，保证模块和驱动的正常加载。

sudo depmod -a

五、安装完毕，如果杀面几个步骤没有出什么问题的话，重新启动机器即可。

六、检查安装效果
 1、首先看看驱动信息是否正确，执行

fglrxinfo

下面的是我结果

display: :0.0 screen: 0
 OpenGL vendor string: ATI Technologies Inc.
 OpenGL renderer string: ATI Radeon HD 2600 XT
 OpenGL version string: 2.1.8201 Release

关键是要有 ATI，不能是其它的，如果不是的话，说明驱动模块没有正确加载，需要根据显示的内容，把对应的驱动删除。

2、看看自己的显卡是否工作在Xv模式下，执行

xvinfo

如果显示的结果很多很多，那就是工作在xv模式下了。如果像下面这样的显示，那还需要再设置

X-Video Extension version 2.2
 screen #0
 no adaptors present

手工设置xv模式

sudo aticonfig –overlay-type=xv

3、看看其它信息

glxinfo | grep direct

我的结果是

direct rendering: Yes

4、测试一下速度和工作是否正常，程序会显示转动的齿轮和一些数值。

glxgears
 fgl_glxgears