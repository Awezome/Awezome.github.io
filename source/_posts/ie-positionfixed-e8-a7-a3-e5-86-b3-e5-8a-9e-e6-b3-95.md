---
title: 'IE position:fixed 解决办法'
id: 1233
categories:
  - JQuery
date: 2014-03-17 16:26:35
tags:
---

> 最近怎么老是写前端。。。
先来个例子：让元素固定在浏览器的底部和距离右边的20个像素
<pre>#top{
position:fixed;
_position:absolute;
bottom:0;
right:20px;
_bottom:auto;
_top:expression(eval(document.documentElement.scrollTop+document.documentElement.clientHeight-this.offsetHeight-(parseInt(this.currentStyle.marginTop,10)||0)-(parseInt(this.currentStyle.marginBottom,10)||0)));
}</pre>
来两个具体的
<!--more-->
<pre class="lang:default decode:true ">//使元素固定在浏览器的顶部
 #top{
_position:absolute;
_bottom:auto;
_top:expression(eval(document.documentElement.scrollTop));
}
//使元素固定在浏览器的底部
 #top{
_position:absolute;
_bottom:auto;
_top:expression(eval(document.documentElement.scrollTop+document.documentElement.clientHeight-this.offsetHeight-(parseInt(this.currentStyle.marginTop,10)||0)-(parseInt(this.currentStyle.marginBottom,10)||0)));
}</pre>
如果固定定位的元素在滚动滚动条的时候会闪动，加入：
<pre> *html{
background-image:url(about:blank);
background-attachment:fixed;
}

</pre>
`//js 方式
&lt;style type="text/css"&gt;
#showTopDiv{position: fixed;_position: absolute;bottom: 0;right:0;z-index:101;height:auto;width:300px;color:#000;padding:10px;border:2px solid #F1D031;background-color: #FFFFA3;}
#showTopDiv a{color:#000;text-decoration:none;position:absolute;right:6px;top:0px;}
&lt;/style&gt;
&lt;span id="showTop"&gt;lalala&lt;/span&gt;
&lt;script type="text/javascript"&gt;
window.onload=function(){
showTop('showTop');
}
function showTop(id){
var divContent=document.getElementById(id).innerHTML;
var divStr="&lt;a href=\"#\" onclick=\"parentNode.style.display='none';\"&gt;x&lt;/a&gt;";
var div =document.createElement("div");
div.id="showTopDiv";
div.innerHTML=divStr+divContent;
document.body.appendChild(div);
}
&lt;/script&gt;

//css 方式
&lt;style type="text/css"&gt;
#showTopDiv{position: fixed;_position: absolute;_bottom:atuo;_top:expression(offsetParent.scrollTop+document.documentElement.clientHeight+document.body.clientHeight-this.offsetHeight-10);bottom: 10;right:0;z-index:101;height:auto;width:300px;color:#000;padding:10px;border:2px solid #F1D031;background-color: #FFFFA3;background-attachment:fixed;background-image:url(about:blank);}
#showTopClose{color:#000;text-decoration:none;position:absolute;right:6px;top:0px;}
&lt;/style&gt;
&lt;div id="showTopDiv"&gt;
&lt;a href="#" id="showTopClose" onclick="parentNode.style.display='none';"&gt;x&lt;/a&gt;
&lt;a href="#"&gt;lalala
&lt;/div&gt;`