---
title: PHP 输出上个月最后一天
tags:
  - php
id: 2043
categories:
  - PHP
date: 2014-05-17 21:58:44
---

上个月第一天很显然可以这样用：
```php
date('Y-m-01', strtotime('-1 month'));
```
上个月最后一天可以这样用
```php
date('Y-m-t', strtotime('-1 month'));
```
这样感觉使用起来很神奇，不过还是strtotime的作用，看看手册上还有哪些用法。
```php
echo strtotime("now"), "\n";
echo strtotime("10 September 2000"), "\n";
echo strtotime("+1 day"), "\n";
echo strtotime("+1 week"), "\n";
echo strtotime("+1 week 2 days 4 hours 2 seconds"), "\n";
echo strtotime("next Thursday"), "\n";
echo strtotime("last Monday"), "\n";
```