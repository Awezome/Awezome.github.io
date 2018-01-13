---
title: Gentoo 安装全过程记录
tags:
  - gentoo
  - Linux &amp; Fedora
id: 1646
categories:
  - Linux
date: 2014-04-08 15:32:57
---

英文安装手册 http://www.gentoo.org/doc/en/gentoo-x86-quickinstall.xml

下载：http://mirrors.163.com/gentoo/releases/x86/current-iso/install-x86-minimal-20140401.iso

引导启动：出现： boot 输入"gentoo nox" , 等一会，出现livecd 则以root 进行livecd

网络连接
输入“ifconfig“回车，如果显示结果中有eth0，则表明已建好了网络连接。如果没有输入”net-setup eth0“回车 ，选择有线连接，自动DHCP

root用户的密码
输入“passwd“ 再输入两次密码

打开ssh
输入“/etc/init.d/sshd start”

分区
Livecd ~ # fdisk /dev/sda
<!--more-->
Command (m for help): 输入n
Command action
extended
primary partition
输入p
Partition number (1-4): 输入1
First cylinder (15-10443, default 15): 默认回车
Last cylinder, +cylinders or +size{K,M,G} (15-10443, default 10443):+100M
以下同理 n p 2 +2G
以下同理 n p 3 默认回车（即默认剩余大小分给root）
设置2为swap
Command (m for help): t
Partition number (1-4): 2
Hex code (type L to list codes): 82
Changed system type of partition 2 to 82 (Linux swap / Solaris)
查看分区
Command (m for help): p
Device Boot    Start    End    Blocks   Id   System
/dev/sda1    1    14    112423+   83   Linux
/dev/sda2    15    146    1060290   82   Linux swap / Solaris
/dev/sda3    147    10443    82710652+   83   Linux
格式化分区
Command (m for help): wq
mkfs.ext3 /dev/sda1
mkswap /dev/sda2
mkfs.ext4 /dev/sda3
挂载分区
swapon /dev/sda2
mount /dev/sda3 /mnt/gentoo
mkdir /mnt/gentoo/boot
mount /dev/sda1 /mnt/gentoo/boot

进入Gentoo的挂载点
# cd /mnt/gentoo

下载Stage Portage
wget http://tel.mirrors.163.com/gentoo/releases/x86/current-iso/stage3-i686-20140401.tar.bz2
# tar xvjpf stage3-*.tar.bz2
wget http://tel.mirrors.163.com/gentoo/snapshots/portage-latest.tar.bz2
# tar xvjf /mnt/gentoo/portage-latest.tar.bz2 -C /mnt/gentoo/usr

配置编译选项
/mnt/gentoo/etc/make.conf
加入
CFLAGS="-O2 -march=i686 -pipe"
CXXFLAGS="${CFLAGS}"

选择境像及rsync站点
# mirrorselect -i -o >> /mnt/gentoo/etc/make.conf
# mirrorselect -i -r -o >> /mnt/gentoo/etc/make.conf

复制DNS信息 （参数"-L"是必须的，用来确保我们拷贝的不是一个符号链接）
# cp -L /etc/resolv.conf /mnt/gentoo/etc/

挂载/proc和/dev
# mount -t proc none /mnt/gentoo/proc
# mount -o bind /dev /mnt/gentoo/dev

chroot到新环境里
# chroot /mnt/gentoo /bin/bash
# env-update
>> Regenerating /etc/ld.so.cache...
# source /etc/profile
# export PS1="(chroot) $PS1"

更新Portage
# emerge --sync
(如果你在使用一个慢速终端比如一些帧缓冲或者是串口的控制台，你可以添加--quiet选项来加速这个过程:)
# emerge --sync --quiet

验证系统profile
# eselect profile list
Available profile symlink targets:
 [1]   default/linux/x86/10.0 *
 [2]   default/linux/x86/10.0/desktop

切换profile （根据实际需求选择）
# eselect profile set 3

USE设置
# nano -w /etc/make.conf
USE="-gtk -gnome qt3 qt4 kde dvd alsa cdr"

指定你的locale
# nano -w /etc/locale.gen
en_US ISO-8859-1
en_US.UTF-8 UTF-8
zh_CN UTF-8
zh_CN GBK

设置时区 （用Hongkong）
# ls /usr/share/zoneinfo
# cp /usr/share/zoneinfo/Hongkong /etc/localtime

安装内核源码 （这一步很慢,如果下载过程中断了，多重试几次）
# emerge gentoo-sources

设置menuconfig
# cd /usr/src/linux
# make menuconfig
File systems --->Pseudo Filesystems --->
    [*] /proc file system support
    [*]tmpfs Virtual memory file system support (former shm fs)
ext3和ext4都必须选上
Device Drivers --->
  Networking Support --->
    <*> PPP (point-to-point protocol) support
    <*>   PPP support for async serial ports
    <*>   PPP support for sync tty ports

复制安装光盘的配置文件
# zcat /proc/config.gz > /usr/share/genkernel/arch/x86/kernel-config

建立kernel
# emerge genkernel
# genkernel all (这一步很慢)

查看内核和initrd的名字
# ls /boot/kernel* /boot/initramfs* （找个记事本记下来，等会有用）
/boot/initramfs-genkernel-x86-3.12.13-gentoo  /boot/kernel-genkernel-x86-3.12.13-gentoo

修改/etc/fstab
# nano -w /etc/fstab
/dev/sda1 /boot   ext3    defaults 1 2
/dev/sda2 none   swap   sw    0 0
/dev/sda3 /    ext4    defaults 0 1

让eth0自动获得IP地址
# nano -w /etc/conf.d/net
加上
config_eth0=( "dhcp" )

自动启用网络
# cd /etc/init.d
# ln -s net.lo net.eth0
# rc-update add net.eth0 default

设置root密码
# passwd

添加一个用户
# useradd –m –G users yourname
# passwd yourname

安装系统日志工具
# emerge syslog-ng
# rc-update add syslog-ng default

安装GRUB，设置引导
# emerge grub

启动GRUB
# grub
grub> root (hd0,0)
grub> setup (hd0)
grub> quit

bash: emergdd: command not found ,刚开始时grub不能用，用了下面这个命令就可以了。
emerge grub-static

创建/boot/grub/grub.conf
# nano -w /boot/grub/grub.conf
输入
default 0
timeout 30
#splashimage=(hd0,0)/boot/grub/splash.xpm.gz
title Gentoo
root (hd0,0)
kernel /boot/kernel-genkernel-x86-3.12.13-gentoo root=/dev/ram0 init=/linuxrc ramdisk=8192 real_root=/dev/sda3 
initrd /boot/initramfs-genkernel-x86-3.12.13-gentoo

exit退出系统环境，reboot重启系统，卸载iso镜像，如果进入不了系统，可以在引导grub界面使用,e编辑，b启动修改grub.conf
OK