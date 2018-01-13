---
title: HTC Mytouch 4G G键改为锁屏键
id: 1023
categories:
  - Other
date: 2012-05-13 10:03:20
tags:
---

. 打开R.E.管理器，打开到/system/usr/keylayout
 2\. 挂载读/写(R/W) ，最上面有个按键，就是左侧文字显示为“挂载为读写（R/W）
 ”，按键上显示“挂载读写（Mount R/O）”
 3\. 长点文件glacier-keypad.kl    ，选择以 “文本编辑方式打开”
 4.key 217  改为POWER    后加 WAKE_DROPPED
    key 183 改为 ENDCALL 后加 WAKE_DROPPED
from:http://bbs.gfan.com/android-3946662-1-1.html