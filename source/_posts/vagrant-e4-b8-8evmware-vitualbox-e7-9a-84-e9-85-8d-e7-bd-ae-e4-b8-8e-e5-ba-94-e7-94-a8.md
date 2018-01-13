---
title: Vagrant 与vmware vitualbox的配置与应用
tags:
  - vagrant
id: 1723
categories:
  - Linux
date: 2014-04-21 09:26:06
---

> Vagrant 与 vmware vitualbox 命令都相同，只是在刚装完Vagrant 后要装一个vm的插件。用vmware的性能理论上会好一些，个人感觉差不多，我使用的是vitualbox。

下载 vitualbox
https://www.virtualbox.org/
下载
Vagrant http://www.vagrantup.com/
下载 box
官方提供的范例：http://files.vagrantup.com/precise32.box
(推荐) http://opscode.github.io/bento/
http://www.vagrantbox.es/<!--more-->
增加 vagrant box add {名称} {box的名字}.box
如vagrant box add fedora fedora-32bit.box

初始化`vagrant init fedora `

启动`vagrant up`

关闭虚拟机`vagrant halt`

删除虚拟机`vagrant destroy`

* * *

下面是安装与卸载vmware插件的命令
Install Fusion Plugin
`vagrant plugin install vagrant-vmware-fusion
vagrant plugin license vagrant-vmware-fusion license.lic`

Install Workstation Plugin
`vagrant plugin install vagrant-vmware-workstation
vagrant plugin license vagrant-vmware-workstation license.lic`

uninstall
`vagrant plugin uninstall vagrant-vmware-fusion`