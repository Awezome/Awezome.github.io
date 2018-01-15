---
title: 'Modern PHP : 传值、引用和对象标识符'
tags:
  - php
id: 2569
categories:
  - PHP
date: 2016-06-05 15:48:48
---

### 对象传值还是引用？

先说个好理解的，boolean、integer、double、string 这些都是传值，传值就是传递过程完全复制一次，如果要对这些类型传引用那么就要加个&。

对于object，默认情况下理解对象是通过引用传递的，但其实这不是完全正确的。真正的是：**从结果来看是类似于传引用，从过程来看是传值。**

对于这个理解，先看几个概念：

### 标识符

准确的讲PHP中没有指针这个概念，都是底层来实现，而**标识符这个概念和指针类似，指向对象真正的内存地址**。一个对象变量已经不再保存整个对象的值，只是保存一个标识符来访问真正的对象内容。

下面用<id>来表示标识符。**将对象赋值给另外一个变量，就是复制一份标识符。**比如创建个一对象，$a=new Bog(), 真正的存在变量里的是标识符：$a=[id]->address ，这一点非常重要。如果赋值给另外一个变量 $b=[id_copy]->address

### 引用

**通常的概念引用和C语言中的指针的类似，不过在PHP中，引用仅仅是变量名的别名，但引用不是指针。**创建一个引用，在PHP 中，变量名和变量内容是不一样的， 因此同样的内容可以有不同的名字。就像 Unix 文件系统中的硬链接，不是软链接。PHP 的引用作用是允许用两个变量来指向同一个内容。

比如 $b=&a，表示为 ($a<=>$b)=[id]->address,这里用$a<=>$b表示引用关系，里面用的是双箭头，$a 和 $b 在这里是完全相同的，这并不是 $a 指向了 $b 或者相反，而是 $a 和 $b 指向了同一个地方，并且$a和$b互为引用，表示改变一个变量，也就是改变了一个变量的[id]，另一个也会改变。

### 引用和标识符关系

下面通过例子来说明引用和标识符的关系
```php
$instance1 = new Test();
$instance2 = $instance1;
$instance3 = & $instance1;

var_dump($instance1 instanceof Test); // True
var_dump($instance2 instanceof Test); // True
var_dump($instance3 instanceof Test); // True
$instance3 = new AnotherTest();
var_dump($instance1 instanceof AnotherTest); // True
var_dump($instance2 instanceof AnotherTest); // False
var_dump($instance3 instanceof AnotherTest); // True
```

$instance3和$instance1互为引用，改变了$instance3，$instance1也会改变。
(1<=>3)=[id]->address
(2)=[id_copy]->address


```php
$instance1 = new Test();
$instance2 = $instance1;
$instance3 = & $instance1;
$instance2 = new AnotherTest();
var_dump($instance1 instanceof AnotherTest); // False
var_dump($instance2 instanceof AnotherTest); // True
var_dump($instance3 instanceof AnotherTest); // False
```
改变2之前
(1<=>3)=[id]->address
(2)=[id_copy]->address
改变2之后，2重新指向了新的[id_new],对应的对象内容也会变
(1<=>3)=[id]->address
(2)=[id_new]->address_new

```php
$instance1 = new StdClass();
$instance2 = $instance1;
$instance1->my_property = 1;
var_dump($instance2->my_property); // Output: 1
```

改变了1的内容，也就是改变了address的内容，1，2标识符指向同一个地址，所以2的内容也会改变。

### 对象作为参数

和赋值给另外一个变量一样，对象作为参数传递，或者作为结果返回，都是同一个标识符的拷贝，不是引用关系。其实就是传的变量的值，这个值是标识符，所以我说过程是传值。但标识符指向同一个地址，修改了指向对象的变量也会都变，所以我说结果看就是类似于传引用。

### array作为参数

PHP传递array时，使用的是copy-on-write机制，也就是array先传引用，当对数组进行更改时再复制一份出来，相当于传值。所以看上去array当作参数时像是传的值。当然如果array想直接传引用，参数前也要加上&。当然不单单是数组作为参数时是写时复制，而在常见的变量赋值也是copy-on-write机制。

http://php.net/manual/zh/language.oop5.references.php
http://stackoverflow.com/questions/8962096/copy-of-object-identifier-and-reference-of-object-identifier-which-one-should
http://php.net/manual/zh/language.references.php#language.references
http://stackoverflow.com/questions/2030906/are-arrays-in-php-passed-by-value-or-by-reference