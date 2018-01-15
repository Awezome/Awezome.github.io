---
title: PHP 导出 CVS Excel
tags:
  - php
id: 2148
categories:
  - PHP
date: 2014-05-27 17:01:39
---

php 5.1 以后自带了一个 fputcsv 函数，如果想把数组导出成excel又没有其它更完善的需求，fputcsv 是个最好的选择，当然要用更强大的功能，可以试试PHPExcel这个库。

下面是导出后并下载的封装函数
```php
function exportCSV($fileName, array $title,array $data) {
  header('Content-Type: application/vnd.ms-excel');
  header("Content-Disposition: attachment;filename = {$fileName}.csv");
  header('Cache-Control: max-age=0');

  $fp = fopen('php://output', 'a');
  fputcsv($fp, $title);

  foreach ($data as $one) {
    fputcsv($fp, $one);
  }
  fclose($fp);
}
```


简单的demo
```php
$title=array('name','age');

$data=array(
array('李四','12'),
array('张三','20')
);

exportCSV('aaa',$title,$data);
```