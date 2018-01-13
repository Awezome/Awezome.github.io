---
title: 强制 IE  Document Mode
tags:
  - ie
id: 1818
categories:
  - Html
date: 2014-05-05 13:32:54
---

接上篇，如果把某个网站加到了ie的兼容性列表里，自然ie 会在ie7 下显示，一些“高级”的样式就会被严重忽视。当然这里会有一些方法来“破解”这些问题，强制以某个版本的 ie 来显示当前的页面。

`<meta http-equiv="X-UA-Compatible" content="IE=7" />
<meta http-equiv="X-UA-Compatible" content="IE=8" />
<meta http-equiv="X-UA-Compatible" content="IE=9" />
<meta http-equiv="X-UA-Compatible" content="IE=10" />`

这些X-UA-Compatible是可以直接无视本地兼容列表和DOCTYPE的，优先级很高。

当然还有一个实用的用法，就是一直以最高的ie mode来渲染。装了ie8就以ie8来渲染，装了ie10就以ie10来渲染。

`<meta http-equiv="x-ua-compatible" content="IE=edge">`

为了更好的支持，不管用哪个版本的浏览器，标记是不能少的。<!--more-->
`<!DOCTYPE html>`
x-ua-compatible 的负面影响还没有研究，做为一个业余的前端开发，真是体会到了ie的事真多。
还有一些高级的用法可以看这里和那里
http://msdn.microsoft.com/en-us/library/ie/jj676916(v=vs.85).aspx
http://msdn.microsoft.com/en-us/library/ie/jj676915(v=vs.85).aspx