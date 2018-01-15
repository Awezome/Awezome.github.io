---
title: 'Modern PHP : 闭包和匿名函数'
tags:
  - php
id: 2548
categories:
  - PHP
date: 2016-05-29 19:05:27
---

### 概念

闭包（closures）是指在创建时封装的状态的函数，匿名函数（Anonymous functions）是指没有名称的函数。在PHP中，闭包和匿名函数被看成一种东西。理论上是不同的，可以理解闭包是由匿名函数构成的一种“结构”。

像string,int等，可以把闭包函数作为变量的值来使用。PHP会自动把此种表达式转换成内置类 Closure 的对象实例。把一个 closure 对象赋值给一个变量的方式与普通变量赋值的语法是一样的，最后也要加上分号。

### 创建闭包

`$anonyFunc = function ($name) {
    return 'Hello ' . $name;
};

echo $anonyFunc->__invoke("Josh");
echo $anonyFunc("Josh");
`

闭包的情况是:

> 1\. 创建一个继承Closure类的闭包对象
>
> 2\. 实现Closure类中的__invoke()方法
>
> 2\. 把闭包赋值给$anonyFunc对象
>
> 3\. 调用变量名后面有()，实际调用__invoke()方法

在array_map中的例子，这种做法更简单，比传统的先定义一个函数再调用要快一点，之前的做法还把回调的实现和使用场所隔离开了。
`<?php
$numbersPlusOne = array_map(function ($number) {
    return $number +1;
}, [1,2,3]);
print_r($numbersPlusOne);
// 输出 --> [2,3,4]`

### use传参

`
<?php
function enclosePerson($name) {
    return function ($doCommand) use ($name) {
        return sprintf('%s, %s', $name, $doCommand);
    };
}
// 将字符串"Clay"封装进闭包
$clay = enclosePerson('Clay');

// 调用闭包
echo $clay('get me sweet tea!');
// 输出 --> "Clay, get me sweet tea!"`
enclosePerson()接收一个$name参数，返回一个封装了$name参数的闭包对象。即使闭包最终离开了enclosePerson()函数的作用域，但是返回的闭包对象$clay中仍然会保留$name参数被附着给闭包时的值。也就说，$name变量仍然存在在闭包中！这就是所说的，**即便闭包所在的环境不存在了，闭包中的封装的状态依然顾存在。**

### bindTo()

闭包有一个方法叫bindTo().这个方法可以吧Closure对象的内部状态绑定到其他对象上。第一个参数是具体的new出来的类变量，**如果要读某个类的protected和private就要把类名用string的形式写到 bindTo 第二个参数上**，当然也可以写成new出来的类变量，PHP会翻译成string的类名，所以说还是直接家string类名吧,如果在class内部的话就直接写__CLASS__喽。这么做之后就可以在匿名函数中使用$this关键字引用重要的应用对象。<!--more-->

`class Foo{
    private $name;
    function __construct($name){
        $this->name = $name;
    }
}
$obj = new Foo('Sam');
$cl = function() {
    return "Hello " . $this->name;
};
$cl = $cl->bindTo($obj, 'Foo');// 'Foo'也可以直接写成$obj
echo($cl());`

bindTo()方法经常被一些PHP框架用来将路由地址映射到匿名回调函数上。这些框架将一个匿名函数绑定到应用程序对象上。可以在匿名函数中使用$this关键词来引用应用程序对象
`
<?php
class App
{
    protected $routes = array();
    protected $responseStatus = '200 OK';
    protected $responseContentType = 'text/html';
    protected $responseBody = 'Hello world';
    
    public function addRoute($routePath, $routeCallback)
    {
        $this->routes[$routePath] = $routeCallback->bindTo($this, __CLASS__);
    }
    
    public function dispatch($currentPath)
    {
        foreach ($this->routes as $routePath => $callback) {
            if ($routePath === $currentPath) {
                $callback();
            }
        }
    
        header('HTTP/1.1 ' . $this->responseStatus);
        header('Content-type: ' . $this->responseContentType);
        header('Content-length: ' . mb_strlen($this->responseBody));
        echo $this->responseBody;
    }
}
`

调用
`
<?php
$app = new App();
$app->addRoute('/users/josh', function () {
    $this->responseContentType = 'application/json;charset=utf8';
    $this->responseBody = '{"name": "Josh"}';
});
$app->dispatch('/users/josh');`

参考：
http://oomusou.io/php/php-bindTo/
《modern php》