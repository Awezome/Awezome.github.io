---
title: 关于 redis 文档 lrange start > end 情况的一点说明
tags:
  - redis
id: 2031
categories:
  - Other
date: 2014-05-13 09:35:50
---

redis 官方文档里对 lrange start > end 说了这样一句话
> If start is larger than the end of the list, an empty list is returned.
> 就是说  start > end 就会返回一个空list ,不过这里还有一种特殊情况。就是end 为负数时，比如
> `lrange ilist 0 -1`
> 那么它会返回整个 list ,这里先不考虑极端情况。就和 an empty list is returned  相冲突，这是其中一种理解方式。另一种是负数在list里不是指小于0的自然数，而是指list尾端的数，比如
> `127.0.0.1:6379> RPUSH lst 1 3 5 7 9
> (integer) 5

127.0.0.1:6379> LRANGE lst 0 -1
1) "1"
2) "3"
3) "5"
4) "7"
5) "9"`

那么此时-1指的是5, 也就符合了 0 <-1 =5 的情况，文档就没有错了。
总的来说，还是文档有要严谨的地方。

p.s. 后来发现 end 包括 stop 及 -1 的情况，文档没有问题。