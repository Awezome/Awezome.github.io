---
title: 博客优化设置记录
id: 1277
categories:
  - Other
date: 2014-03-19 16:10:50
tags:
---

本来空间响应不够快，又加上之前没有很好的设置，网站打开的速度都过了1s。主要是把前端的代码减少了体积，库更少的引用。

1.  去掉了无用的js库
2.  精简了php的逻辑
3.  关掉了七牛cdn, 只保留图片存储功能
4.  去掉了代码高亮，简单重写了&lt;code&gt;
5.  使用 Gravatar缓存
6.  使用WP Super Cache大牛插件
7.  加上了*.eot的缓存时间
8.  侧边栏<span style="line-height: 1.5em;">Recent, </span><span style="line-height: 1.5em;">Popular, </span><span style="line-height: 1.5em;">Comment, </span><span style="line-height: 1.5em;">Tag </span>在ie下不在一行
9.  顶部菜单栏的固定
10.  背景的固定
11.  去掉了tweet
12.  试试百度加速乐。。。