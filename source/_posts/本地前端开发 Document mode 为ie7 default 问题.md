---
title: 本地前端开发 Document mode 为ie7 default 问题
tags:
  - ie
id: 1787
categories:
  - Html
date: 2014-05-05 13:16:47
---

一直在本地默默的修改另一套 WordPress 主题，本地的环境都ok, 但是用ie 跑的时候确一直是图中的丑样。查看 Source &lt;!DOCTYPE html&gt; 局然是被注释掉了，难到是不知写了什么影响了？

[![1](/wp-content/uploads/2014/05/e8059811450b854a7b77cc653761282d.png)](/wp-content/uploads/2014/05/e8059811450b854a7b77cc653761282d.png)


接下来查看Document mode局然是ie7 default , 当然对于ie7 来说不支持rgba 的css标签。

[![2](/wp-content/uploads/2014/05/1102774157837fdba32b0df0811ab9ea.png)](/wp-content/uploads/2014/05/1102774157837fdba32b0df0811ab9ea.png)


下面的一行字到时引起了我的注意。Via local compatibility view settings. 本地的兼容列表设置-检查一下：<span style="color: #454545;">Alt –&gt; Tools –&gt; Compatibility View Settings</span><!--more-->

[![3](/wp-content/uploads/2014/05/3447418403fc4780091e5a804471a00f.png)](/wp-content/uploads/2014/05/3447418403fc4780091e5a804471a00f.png)


okay ，把127.0.0.1 remove 掉，刷新,查看 Document mode:

[![4](/wp-content/uploads/2014/05/b969542c2f6cba75156ffed773cf5801.png)](/wp-content/uploads/2014/05/b969542c2f6cba75156ffed773cf5801.png)

查看 Source


[![5](/wp-content/uploads/2014/05/3534b2282023a8d4926ef7a7cd09532c.png)](/wp-content/uploads/2014/05/3534b2282023a8d4926ef7a7cd09532c.png)

搞定。&lt;!DOCTYPE html&gt; 也正常了。

[![6](/wp-content/uploads/2014/05/d875d51eba70514e60c5ab64c06ab787.png)](/wp-content/uploads/2014/05/d875d51eba70514e60c5ab64c06ab787.png)
