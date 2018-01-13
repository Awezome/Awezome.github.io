---
title: 几个有趣的PHP字符串和数字比较时强制转换的例子
tags:
  - php
id: 2058
categories:
  - PHP
date: 2014-05-21 15:30:41
---

其实也不用多讲什么，看看几个特殊的例子就会很明白，结果可以自行写写试试。注意不同点，官网链接给个 http://cn2.php.net/manual/en/language.operators.comparison.php

`var_dump(0 == "0");
var_dump(0 === "0");
var_dump(0 == "aaa");
var_dump("0" == "aaa");
var_dump(5 == "5");
var_dump(5 === "5");
var_dump(5 > "4");
var_dump("1" == "01");
`
<!--more-->

`switch ("aaaa") {  
case 'bbbb':  
    echo "it is bbbb";  
    break;  
case "aaaa": 
    echo "it is aaaaa";  
    break;  
}`

`switch ("aaaa") {  
case '0':  
    echo "it is '0'";  
    break;  
case "aaaa": 
    echo "it is aaaaa";  
    break;  
}  `

`switch ("aaaa") {  
case 0:  
    echo "it is 0";  
    break;  
case "aaaa": 
    echo "it is aaaaa";  
    break;  
}  
`