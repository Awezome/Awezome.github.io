---
title: php strstr stristr strpos
tags:
  - php
id: 1708
categories:
  - PHP
date: 2014-04-16 17:44:23
---

string strstr ( string $haystack , mixed $needle [, bool $before_needle = false ] )
返回 haystack 字符串从 needle 第一次出现的位置开始到 haystack 结尾的字符串。
Note:
该函数区分大小写。如果想要不区分大小写，请使用 stristr()。
Note:
如果你仅仅想确定 needle 是否存在于 haystack 中，请使用速度更快、耗费内存更少的 strpos() 函数。
from : http://www.php.net/manual/zh/function.strstr.php