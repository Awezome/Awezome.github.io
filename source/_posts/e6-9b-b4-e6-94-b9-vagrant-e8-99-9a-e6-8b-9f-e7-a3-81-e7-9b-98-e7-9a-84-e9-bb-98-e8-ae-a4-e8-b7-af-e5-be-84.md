---
title: 更改 vagrant 虚拟磁盘的默认路径
tags:
  - vagrant
id: 1995
categories:
  - Linux
date: 2014-05-11 21:05:28
---

至少在我的电脑上 vagrant 默认是放在了 c:/user 里，本来很小的系统盘在 vagrant add box 后就更小了，然后再加上 virtual box 也放在 系统盘里，一下就多了好几个G。最直接的方法当然是把他们给移到其它盘去。

在这之前，要简单说一下 vagrant 的几个命令后的到底动了哪几个大文件。

### 一点原理

首先我们会用 vagrant add box ，这个是把下载好的比如 fedora.box 文件加载到
C:\Users\user_name\.vagrant.d\boxes 里，并解成 packer-fedora-20-i386-disk1.vmdk 类似的 virtual box 可识别的文件，当然还有一些其它的像 Vagrantfile 配置文件。也就是说有多少种不同的 add box 就会生成多少个 vmdk ，那当然要占空间了。

接下来我们会 vagrant init , 这个没大文件，就是在当前目录下生成个 Vagrantfile。不过对于第一次 vagrant up就会在 C:\Users\user_name\.VirtualBox\VirtualBox VMs 生成过G的 vmdk ，这个是直接具体的实例。<!--more-->

对于上面讲的简单说就是 add box 是根据 .box 生成一个模板，放在 .vagrant.d\boxes ，以后每次 init &amp; up 时就会再用这个模板再生成具体的实例，放在.VirtualBox\VirtualBox VMs 。也就是说我们要把 .vagrant.d VirtualBox VMs 给挪了。

下面是具体的操作

### 更改 VirtualBox 虚拟机 VirtualBox VMs 文件的位置

打开 VirtualBox ，Ctrl+G, 将默认虚拟电脑位置改为其他磁盘下的路径，并将原路径 C:\Users\user_name\.VirtualBox\VirtualBox VMs 下的文件移动到新路径下。

### 更改 vagrant 配置文件 .vagrant.d 位置

说到这我有一个疑问，官方的给的方法是新建环境变量VAGRANT_HOME，指向新的路径，并将 C:\Users\user_name\.vagrant.d 移动到新的位置。这招好像 linux 、mac 都好使，但是我的机器 win8.1 x64 好像不好使，我的方法是直接改原文件。

在这里，这个具体版本具体安装路径自己改
`D:\HashiCorp\Vagrant\embedded\gems\gems\vagrant-1.5.3\lib\vagrant\environment.rb `，
第 119 行改成
`@home_path = Util::Platform.fs_real_path("D:/vagrant/home/")`
然后再把 C:\Users\user_name\.vagrant.d 里的文件移到 D:/vagrant/home/ 里面，重启 vagrant 就可以了。

另外说一句，先备份，再操作。