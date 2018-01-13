---
title: dex2jar出现Error occurred during initialization of VM解决办法
tags:
  - dex2jar
  - java
id: 1543
categories:
  - Java
date: 2014-03-27 11:18:27
---

使用dex2jar出现
`Error occurred during initialization of VM Could not reserve enough space for object heap`
这人肯定是内存不够了呗，打开dex2jar.bat，减少heap size为-Xms128m -Xmx256m就可以了。