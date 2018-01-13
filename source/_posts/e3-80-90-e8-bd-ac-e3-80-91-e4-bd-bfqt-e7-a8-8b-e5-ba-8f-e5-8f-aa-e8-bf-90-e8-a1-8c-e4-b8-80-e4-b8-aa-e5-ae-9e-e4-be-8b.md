---
title: 【转】使Qt程序只运行一个实例
id: 1064
categories:
  - Qt
date: 2012-08-05 18:12:08
tags:
---

【转】[http://blog.csdn.net/tingsking18/](http://blog.csdn.net/tingsking18/)

让应用程序只运行一个实例，这个问题很古老了。以及以前 HGR 老胡写过操作 event 的 delphi 版本的。当然在 win 下这样的解决方案还是很多的。

让 Qt 程序只运行一个实例，当然用 win 下的 native API 是很不靠谱的，因为这样会牺牲掉 Qt 跨平台的特性。所以我给出下面两种解决方案。原理上就是进程间通讯。 QSingleApplication 用的而是 socket ，而我使用的是共享内存。

1\. 使用 QSingleApplication 。

QSingleApplication 是 Qt 提供的一个 solution ，它不包含在 Qt 的 library 中。遵循 LGPL 协议。关于如何使用，下载了这个 solution 之后，里面有例子。还有， QtCreator 中还用到了它。你也可以翻一番 QtCreator 的源代码。

2\. 使用共享内存。

// 确保只运行一次

QSystemSemaphore sema("JAMKey",1,QSystemSemaphore::Open);

sema.acquire();// 在临界区操作共享内存 SharedMemory<!--more-->

QSharedMemory mem("SystemObject");// 全局对象名

if (!mem.create(1))// 如果全局对象以存在则退出

{

QMessageBox::information(0, MESSAGEBOXTXT,"An instance has already been running.");

sema.release();// 如果是 Unix 系统，会自动释放。

return 0;

}

sema.release();// 临界区