---
title: 慎用tcp_tw_recycle
tags:
  - Linux
categories:
  - Linux
date: 2018-05-13 09:30:50
---

```
echo 1 > /proc/sys/net/ipv4/tcp_tw_recycle
sysctl -a | grep "tcp_tw"
sysctl -w net.ipv4.tcp_tw_recycle=0
sysctl -p
```