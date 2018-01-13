---
title: Flash声音处理基础AS代码
id: 913
categories:
  - Other
date: 2012-02-24 13:10:38
tags:
---

var mysound:Sound = new Sound();

//让mysound具有Sound类的属性和方法

mysound.attachSound("Cannon");

//mysound链接到库中名为"Cannon"的声音元件

Start_Point = 0;

//设置播放的起点位置

play_btn.onRelease = function() {
mysound.start(Start_Point/1000);};

//播放按钮：从起点位置开始播放声音，因为要接收秒数，所以要除1000

pause_btn.onRelease = function() {
Start_Point = mysound.position;
mysound.stop();};

//暂停按钮：先保存当前播放到的位置为起点，然后停止播放

stop_btn.onRelease = function() {<!--more-->
Start_Point = 0;
mysound.stop();};

//停止按钮：将起点位置设为0，然后停止播放

Flash充电：Sound类常用属性和方法

（1）Sound.attachSound("idName"):声音对象依附声音元件

mysound.attachSound("Cannon")

（2）Sound.start(播放起点,循环次数)：开始播放声音

mysound.start(5,3)

//从5秒钟位置开始播放，循环三次

mysound.start()

//从0秒钟位置开始播放，循环一次

（3）Sound.stop("idName"):停止播放声音

mysound.stop()

//停止全部声音

（4）Sound.setVolume(1～100)：设定音量

mysound.setVolume(50)

//设置音量为50

（5）Sound.getVolume():取得音量设定值

mysound.setVolume(33)

trace(mysound.getVolume())

//返回33