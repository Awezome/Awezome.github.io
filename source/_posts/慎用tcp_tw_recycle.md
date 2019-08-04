---
title: 被移除的 tcp_tw_recycle
id: abolish-tcp-tw-recycle
tags:
  - Linux
categories:
  - Linux
date: 2018-05-13 09:30:50
---

之前遇到一个问题，访问线上某台机器总是在一断时间出现连不上的情况，查了好久才发现是 tcp_tw_recycle 这个参数被设置成了1。

NAT网络前的client经过NAT处理后，对服务端来说看上去IP+端口都一样，client会携带各自的timestamp值并非是递增的，开启tcp_tw_recycle后，会丢弃timestamp值不符合递增条件的数据包，这样有的连接就直接被断掉了，造成无法访问的场景。

### 最佳实践
net.ipv4.tcp_timestamps=1
net.ipv4.tcp_tw_recycle=0
net.ipv4.tcp_tw_reuse=1  //开启可以重用TIME_WAIT套接字


### 查看与设置
Linux 使sysctl来查看和设置内核参数

```
sysctl -a | grep "tcp_tw" 
echo 1 > /proc/sys/net/ipv4/tcp_tw_recycle
sysctl -w net.ipv4.tcp_tw_recycle=0
sysctl -p
```


Linux 4.12 已经移除了 tcp_tw_recycle 这个参数，https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=95a22caee396cef0bb2ca8fafdd82966a49367bb 

### Ref.
https://vincent.bernat.ch/en/blog/2014-tcp-time-wait-state-linux
https://ieevee.com/tech/2017/07/19/tcp-tw-recycle.html
http://perthcharles.github.io/2015/08/27/timestamp-NAT/
