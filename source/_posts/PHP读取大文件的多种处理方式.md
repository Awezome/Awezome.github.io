---
title: PHP读取大文件的多种处理方式
tags:
  - php
  - 大文件
id: 2181
categories:
  - PHP
date: 2014-07-27 17:55:23
---

一直感觉PHP处理大文件是一件很不靠谱的事性，幸好有着越来越多的方法来解决这件事行。

1.直接File
如果这个大文件不算“太大”，那么就可以先用File这个方法直接把文件全部读到变量里，成为一个数组，每行为一个value。优点就是简单快速。

```php
<?php
print_r(file("test.txt"));
?> 
//output
Array
(
[0] => Hello World. Testing testing!
[1] => Another day, another line.
[2] => If the array picks up this line,
[3] => then is it a pickup line?
)
```

2.这个方法是很传统的方法，每读一行处理一行。<!--more-->
```php
$handle = @fopen($filename, "r");
if (!$handle) {
    exit();
}
$i=0;
while (($buffer = fgets($handle)) !== false) {
    echo $buffer;
}
if (!feof($handle)) {
    echo ("Error: unexpected fgets() fail\n");
    exit();
}
fclose($handle);
```

3.使用SPL SplFileObject , 这个好像更厉害，不过要PHP 5.1的支持。
这个类用来对文本文件进行遍历。
```php
<?php
try{
    foreach( new SplFileObject('/log/php.log') as $line)
    echo $line.'
';
}catch (Exception $e){
    echo $e->getMessage();
}
?>
```
返回文本文件的第10行，可以这样写：
```php
<?php
try{
    $file = new SplFileObject("/log/php.log");
    $file->seek(10);
    echo $file->current();
}catch (Exception $e){
    echo $e->getMessage();
}
?>
```

当然要判断当前行数是不是超过了文件总行数，获得文件的行数的方汗以。
```php
<?php
$fp = fopen($temp_file ,'r') or die("open file failure!");
$total_line = 0;
if($fp){
    /* 获取文件的一行内容，注意：需要php5才支持该函数; */
    while(stream_get_line($fp, 8192, "\r\n")){
        $total_line++;
    }
    fclose($fp);
}
?>
```

这里有个集2，3方法的函数，可以读取几行的内容
```php
public function getFileLines($filename, $startLine = 1, $endLine=50, $method='rb') {
    $content = array();
    $count = $endLine - $startLine;
    if(version_compare(PHP_VERSION, '5.1.0', '>=')){/* 判断php版本（因为要用到SplFileObject，PHP>=5.1.0） */
        $fp = new SplFileObject($filename, $method);
        $fp->seek($startLine-1);/* 转到第N行, seek方法参数从0开始计数 */
        for($i = 0; $i <= $count; ++$i) {
            $content[] = unserialize($fp->current());/* current()获取当前行内容 */
            $fp->next();/* 下一行 */
        }
    }else{/*PHP<5.1 */
        $fp = fopen($filename, $method);
        if(!$fp) return 'error:can not read file';
        for ($i=1;$i<$startLine;++$i) {/* 跳过前$startLine行 */
            fgets($fp);
        }
        for($i;$i<=$endLine;++$i){
            $content[] = unserialize(fgets($fp));/* 读取文件行内容 */
        }
        fclose($fp);
    }
    return array_filter($content); /* array_filter过滤：false,null,'' */
}
```

相关阅读：http://www.redyun.net/technology/101.html