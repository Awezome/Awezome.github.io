---
title: 'Modern PHP : 生成器及实例'
tags:
  - php
id: 2563
categories:
  - PHP
date: 2016-06-02 15:42:27
---

### 概念

PHP中的生成器（Generator）说简单点就是一种特殊的迭代器（Iterator），不过与标准的PHP迭代器不同，标准的迭代器通常都是在内存中全部计算过的数据集进行迭代，而生成器可以动态的计算并交出下一个值，这样可以不占用宝贵的系统内存。因为生成器是 forward-only 的迭代，在迭代开始后不能 rewind ，所以同一个生成器不能迭代多次。

### yield

使用生成器要用到yield关键字。yield 与 return 相似，不同的是 yield 不会终止函数的执行，而是为循环提供一个值并暂停生成器函数的执行。
`<?php
function myGenerator() {
    yield 'value1';
    yield 'value2';
    yield 'value3';
}

foreach (myGenerator() as $yieldedValue) {  
    echo $yieldedValue, PHP_EOL;  
}  
`

当一个生成器被调用的时候，它返回一个可以被遍历的对象.当你遍历这个对象的时候(例如通过一个foreach循环)，PHP 将会在每次需要值的时候调用生成器函数，并在产生一个值之后保存生成器的状态，这样它就可以在需要产生下一个值的时候恢复调用状态。一旦不再需要产生更多的值，生成器函数可以简单退出，而调用生成器的代码还可以继续执行，就像一个数组已经被遍历完了。

### 对比

再看一个传统生成随机数的例子：
<!--more-->
`<?php
function makeRange($length) {
    $dataset = [];
    for ($i = 0; $i < $length; $i++) {
        $dataset[] = $i;
    }

    return $dataset;
}

$customRange = makeRange(1000000);
foreach ($customRange as $i) {
    echo $i, PHP_EOL;
}`

上面的例子中makeRange函数为数组预先分配了一百万个整型的内存区域。而PHP生成器同样可以完成上面的功能，而任何时候都只需要分配一个整型的内存，看看例子

`<?php
function makeRange($length) {
    for ($i = 0; $i < $length; $i++) {
        yield $i;
    }
}

foreach (makeRange(1000000) as $i) {
    echo $i, PHP_EOL;
}
`

### key/value对

yield 也可以生成 key/value对，与数组类似。

`<?php
$input = <<<'EOF'
1;PHP;Likes dollar signs
2;Python;Likes whitespace
3;Ruby;Likes blocks
EOF;

function input_parser($input) {
    foreach (explode("\n", $input) as $line) {
        $fields = explode(';', $line);
        $id = array_shift($fields);

        yield $id => $fields;
    }
}

foreach (input_parser($input) as $id => $fields) {
    echo "$id:\n";
    echo "    $fields[0]\n";
    echo "    $fields[1]\n";
}
`

### 注意点

如果在一个表达式上下文(例如在一个赋值表达式的右侧)中使用yield，你必须使用圆括号把yield申明包围起来。
`$data = (yield $value);`

yield也可以在没有参数传入的情况下被调用来生成一个 NULL值并配对一个自动的键名。

要注意，一个生成器不可以返回值，这样做会产生一个编译错误。然而return空是一个有效的语法并且它将会终止生成器继续执行。

### 实践

再实践中，我们可以用生成器读取文件并处理，当然也可以把业务逻辑写在while里面，不过这样的话会很乱，如果有多个读文件的逻辑要写多个类似的方法，所以用生成器，既可以把功能和业务分开，也可以减少内存使用。
`    function read_file_line($file){
        $handle=@fopen($file,'r');
        if(!$handle){
            throw new Exception("Can not read file!");
        }

        while(($line=fgets($handle))!==false){
            yield $line;
        }

        if (!feof($handle)) {
            throw new Exception('Error: unexpected fgets() fail');
        }
        fclose($handle);
        return ;
    }

        foreach (read_file_line($file) as $row) {
            //print_r($row);
        }

`

再看看如何读csv:

`<?php
function getRows($file) {
    $handle = fopen($file, 'rb');
    if ($handle === false) {
        throw new Exception();
    }
    while (feof($handle) === false) {
        yield fgetcsv($handle);
    }
    fclose($handle);
}

foreach (getRows('data.csv') as $row) {
    print_r($row);
}`

### send()

最后还有补充一点
生成器可以通过send()函数来注入值，通过yield来接收。然后可以像其他生成器函数中的值那样使用。

`function printer() {
    while (true) { 
        // 通过 yield 语句返回注入的值
        $string = yield;
        echo $string;
    }
}

$printer = printer();
$printer->send('Hello world!'); // 输出 Hello world!`

参考：
www.powerxing.com/php-review-generator/
www.cnblogs.com/CraryPrimitiveMan/p/5130056.html
《modern php》