---
title: 'JQuery ready , load和windows.load的关系'
id: 1225
categories:
  - JQuery
date: 2014-03-17 09:36:39
tags:
---

> 前两天遇到一个问题：页面需要弹出一个dialog, dialog里要执行一个显示tips, 在chrome显示没有问题，但是到了firefox下就一直不能显示，后来才知道，加载这个tips插件时页面内容没有显示全，页面内容是通过json读的后台，所以不能显示。
默认调用这个显示tips的函数是用 jQuery.ready() 后来换成 windows.load就可以了，实在不行就再加个时间：
`
window.onload=function(){
setTimeout('joyride()',200);
}
`
这里找到了关于这几个函数的理解说明：

window.onload方法是在网页中所有的元素（包括元素的所有关联文件）完全加载到浏览器后才执行，即JavaScript此时才可以访问网页中的任何元素。

过jQuery中的$(document).ready()方法注册的事件处理程序，在DOM完全就绪时就可以被调用。只要DOM就绪就会被执行，因此可能此时元素的关联文件末下载完。<!--more-->

jQuery load()方法会在元素的onload事件中绑定一个处理函数。如果处理函数绑定给window对象，则会在所有内容（包括窗口、框架、对象和图像等）加载完毕后触发，如果处理函数绑定在元素上，则会在元素的内容加载完毕后触发。
`
$(window).load(function(){
// 编写代码
})
`
等价于JavaScript中的以下代码：
`
window.onload = function(){
// 编写代码
})
`
另外：
`
$(document).ready(function(){
// 编写代码
})

$(function(){
// 编写代码
})

$().ready(function(){
// 编写代码 当$()不带参数时，默认参数就是“document”
})
`
是相同

详细：http://www.nowamagic.net/librarys/posts/jquery/71