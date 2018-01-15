---
title: 纯 PHP (Html) + Css 实现组织结构图
tags:
  - css
  - html
  - php
id: 1967
categories:
  - PHP
date: 2014-05-09 14:43:07
---

网上有很多开源的js版本的组织结构图工具，不过假设有这么个场景，有一个10多m的xml文件，里面是组织关系，要用php解析，再到js生成，这个两个过程都是很费时的，尤其是js的渲染过程，大部分的js版本都是再生成div的方式，这肯定会更加的慢了。

我的方法是，直接用php输出一个相应的html结构，我用的是一定结构的table,再通过css画画线就搞定了。具体的实现方法直接看代码就ok了。有问题可以讨论。

github : [https://github.com/Awezome/PHP-to-OrgChart](https://github.com/Awesomez/PHP-to-OrgChart)

![sreenshot](https://raw.githubusercontent.com/Awesomez/PHP-to-OrgChart/master/sreenshot.PNG)


纯Html实现看这里
https://github.com/Awezome/PHP-to-OrgChart/blob/master/demo/pureHtml.html
通过php数组生成看这里
https://github.com/Awezome/PHP-to-OrgChart/blob/master/demo/simple.php

php的使用方法
```php
include '../src/PHPtoOrgChart.php';
$data=array(
    'a'=>array(
        'aa'=>array(
            'aaa'=>'Mike',
            'aab'=>'Look',
            'aac'=>'Rum',
        ),
        'bb'=>array(
            'aaa'=>'123',
            'aab'=>'567',
            'aac'=>'890',
            'bbdd'=>array(
                'aaa'=>'123',
                'aab'=>'567',
                'aac'=>'890',
            ),
        ),
    ),
    'b'=>array(
        'cc'=>array(
            'aaa'=>'Mike',
            'aab'=>'Look',
            'aac'=>'Rum',
        ),
        'dd'=>array(
            'aaa'=>'123',
            'aab'=>'567',
            'aac'=>'890',
        ),
    ),
);
echo '<div class="orgchart">';
PHPtoOrgChart($data);
echo '</div>';
```