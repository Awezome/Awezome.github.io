---
title: htmlspecialchars 和 htmlentities 的区别
id: 529
categories:
  - PHP
date: 2012-01-13 22:16:39
tags:
---

PHP 手册中对 htmlspecialchars 写道

> The translations performed are:
> '&' (ampersand) becomes '&amp;'
> '"' (double quote) becomes '&quot;' when ENT_NOQUOTES is not set.
> ''' (single quote) becomes '&#039;' only when ENT_QUOTES is set.
> '<' (less than) becomes '&lt;'
> '>' (greater than) becomes '&gt;'

htmlspecialchars 只转化上面这几个html代码，而 htmlentities 却会转化所有的html代码，连同里面的它无法识别的中文字符也给转化了。

我们可以拿一个简单的例子来做比较：
> $str='[测试页面](test.html)';
> echo htmlentities($str);
> // &lt;a href=&quot;test.html&quot;&gt;&sup2;&acirc;&Ecirc;&Ocirc;&Ograve;&sup3;&Atilde;&aelig;&lt;/a&gt;
> 
> $str='[测试页面](test.html)';
> echo htmlspecialchars($str);
> // &lt;a href=&quot;test.html&quot;&gt;测试页面&lt;/a&gt;
<!--more-->

结论是，有中文的时候，最好用 htmlspecialchars ，否则可能乱码

另外参考一下这个自定义函数

> function my_excerpt( $html, $len ) {
>     // $html 应包含一个 HTML 文档。
>     // 本例将去掉 HTML 标记，javascript 代码
>     // 和空白字符。还会将一些通用的
>     // HTML 实体转换成相应的文本。
>     $search = array ("'<script[^>]*?>.*?</script>'si",  // 去掉 javascript
>                     "'<[/!]*?[^<>]*?>'si",           // 去掉 HTML 标记
>                     "'([rn])[s]+'",                 // 去掉空白字符
>                     "'&(quot|#34);'i",                 // 替换 HTML 实体
>                     "'&(amp|#38);'i",
>                     "'&(lt|#60);'i",
>                     "'&(gt|#62);'i",
>                     "'&(nbsp|#160);'i",
>                     "'&(iexcl|#161);'i",
>                     "'&(cent|#162);'i",
>                     "'&(pound|#163);'i",
>                     "'&(copy|#169);'i",
>                     "'&#(d+);'e");                    // 作为 PHP 代码运行
>     $replace = array ("",
>                      "",
>                      "1",
>                      """,
>                      "&",
>                      "<",
>                      ">",
>                      " ",
>                      chr(161),
>                      chr(162),
>                      chr(163),
>                      chr(169),
>                      "chr(1)");
>     $text = preg_replace ($search, $replace, $html);
>     $text = trim($text);</code>
>     return mb_strlen($text) >= $len ? mb_substr($text, 0, $len) : '';
> }