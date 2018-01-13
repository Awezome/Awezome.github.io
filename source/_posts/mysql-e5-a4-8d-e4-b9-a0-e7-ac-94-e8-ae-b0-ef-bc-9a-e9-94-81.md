---
title: MySQL复习笔记：锁
tags:
  - MySQL
id: 2535
categories:
  - MySQL
date: 2015-05-15 17:50:17
---

MySQL的锁系统：shared lock和exclusive lock（共享锁和排他锁，也叫读锁和写锁，即read lock和write lock）

表上的write lock会阻塞其他会话中write lock 和 read lock
表上的read lock只会阻塞其他会话中write lock，而不会阻塞read lock

MySQL有三种锁的级别：页级、表级、行级。

MyISAM和MEMORY存储引擎采用的是表级锁（table-level locking）；BDB存储引擎采用的是页面锁（page-level locking），但也支持表级锁；InnoDB存储引擎既支持行级锁（row-level locking），也支持表级锁，但默认情况下是采用行级锁。

MySQL这3种锁的特性可大致归纳如下：
表级锁：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高,并发度最低。
行级锁：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低,并发度也最高。
页面锁：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。

快照读：简单的select操作，属于快照读，不加锁。(当然，也有例外，下面会分析)
`select * from table where ?;`
<!--more-->

当前读：特殊的读操作，插入/更新/删除操作，属于当前读，需要加锁。
`select * from table where ? lock in share mode; //在读取的行上设置一个共享模式的锁。这个共享锁允许其它session读取数据但不允许修改它。 行读取的是最新的数据，如果他被其它事务使用中而没有提交，读取锁将被阻塞直到那个事务结束。
select * from table where ? for update; //在读取行上设置一个排他锁。组织其他session读取或者写入行数据
insert into table values (…);
update table set ? where ?;
delete from table where ?;`

当Update SQL被发给MySQL后，MySQL Server会根据where条件，读取第一条满足条件的记录，然后InnoDB引擎会将第一条记录返回，并加锁 (current read)。待MySQL Server收到这条加锁的记录之后，会再发起一个Update请求，更新这条记录。一条记录操作完成，再读取下一条记录，直至没有满足条件的记录为止。因此，Update操作内部，就包含了一个当前读。同理，Delete操作也一样。Insert操作会稍微有些不同，简单来说，就是Insert操作可能会触发Unique Key的冲突检查，也会进行一个当前读。
针对一条当前读的SQL语句，InnoDB与MySQL Server的交互，是一条一条进行的，因此，加锁也是一条一条进行的。先对一条满足条件的记录加锁，返回给MySQL Server，做一些DML操作；然后在读取下一条加锁，直至读取完毕。

from http://my.oschina.net/xinxingegeya/blog/296618