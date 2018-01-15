---
title: PHP时间戳
id: 1092
categories:
  - PHP
date: 2012-11-06 15:27:51
tags:
---

```php
$t = time();
$t1 = mktime(0,0,0,date(“m”,$t),date(“d”,$t),date(“Y”,$t));
$t2 = mktime(0,0,0,date(“m”,$t),1,date(“Y”,$t));
$t3 = mktime(0,0,0,date(“m”,$t)-1,1,date(“Y”,$t));
$t4 = mktime(0,0,0,1,1,date(“Y”,$t));
$e1 = mktime(23,59,59,date(“m”,$t),date(“d”,$t),date(“Y”,$t));
$e2 = mktime(23,59,59,date(“m”,$t),date(“t”),date(“Y”,$t));
$e3 = mktime(23,59,59,date(“m”,$t)-1,date(“t”,$t3),date(“Y”,$t));
$e4 = mktime(23,59,59,12,31,date(“Y”,$t));
//测试
echo date(“当前 Y-m-d H:i:s”,$t).” $t
”;
echo date(“今天起点 Y-m-d H:i:s”,$t1).” $t1
”;
echo date(“今月起点 Y-m-d H:i:s”,$t2).” $t2
”;
echo date(“上月起点 Y-m-d H:i:s”,$t3).” $t3
”;
echo date(“今年起点 Y-m-d H:i:s”,$t4).” $t4
”;
//测试
echo date(“今天终点 Y-m-d H:i:s”,$e1).” $e1
”;
echo date(“今月终点 Y-m-d H:i:s”,$e2).” $e2
”;
echo date(“上月终点 Y-m-d H:i:s”,$e3).” $e3
”;
echo date(“今年终点 Y-m-d H:i:s”,$e4).” $e4
”;
```
结果：
当前 2011-05-24 15:42:55 1306222975
今天起点 2011-05-24 00:00:00 1306166400
今月起点 2011-05-01 00:00:00 1304179200
上月起点 2011-04-01 00:00:00 1301587200
今年起点 2011-01-01 00:00:00 1293811200
今天终点 2011-05-24 23:59:59 1306252799
今月终点 2011-05-31 23:59:59 1306857599
上月终点 2011-04-30 23:59:59 1304179199
今年终点 2011-12-31 23:59:59 1325347199

```php
echo "今天:".date("Y-m-d")."";
echo "昨天:".date("Y-m-d",strtotime("-1 day")), "";
echo "明天:".date("Y-m-d",strtotime("+1 day")). "";
echo "一周后:".date("Y-m-d",strtotime("+1 week")). "";
echo "一周零两天四小时两秒后:".date("Y-m-d G:H:s",strtotime("+1 week 2 days 4 hours 2 seconds")). "";
echo "下个星期四:".date("Y-m-d",strtotime("next Thursday")). "";
echo "上个周一:".date("Y-m-d",strtotime("last Monday"))."";
echo "一个月前:".date("Y-m-d",strtotime("last month"))."";
echo "一个月后:".date("Y-m-d",strtotime("+1 month"))."";
echo "十年后:".date("Y-m-d",strtotime("+10 year"))."";
```