---
title: 几种常见的排序算法的PHP实现
tags:
  - 算法
id: 2050
categories:
  - Algorithm
date: 2014-05-19 10:03:30
---

```php
static function Bubble($arr) {
$num = count($arr);
for ($i = 1; $i < $num; $i++) {
    for ($j = $num - 1; $j >= $i ; $j--) {
        if ($arr[$j] < $arr[$j-1]) {
            self::exch($arr,$j,$j-1);
        }
    }
}
return $arr;
}
```

```php
static public function selection (&$data){
    $size=count($data);
    for($i=0;$i<$size;$i++){
        $min=$i;
        for($j=$i+1;$j<$size;$j++){
            if($data[$j]<$data[$min]){
                $min=$j;
            }
        }
        self::exch($data,$i,$min);
    }  
}
```

```php
static function insertion(&$data){
    $size=count($data);
    for($i=1;$i<$size;$i++){
        for($j=$i;$j>0 $$ $a[$j]<$a[$j-1];$j--){
            self::exch($data,$j,$j-1);
        }
    }
}
```
```php
static function shell(&$data){
    $size=count($data);
    $h=1;
    while($h<$size/3){
        $h=3*$h+1;
    }            
    while($h>=1){
        for($i=$h;$i<$size;$i++){
            for($j=$i;$j>=$h && $data[$j]<$data[$j-$h];$j-=$h){
                self::exch($data,$j,$j-$h);
            }
        }
        $h=$h/3;
    }
}
```
```php
static function quick(array &$data,$lo,$hi){
    if($hi<=$lo){
        return ;
    }
    
    //partition
    $i=$lo;
    $j=$hi+1;
    $v=$data[$lo];
    while(1){
        while($data[++$i]<$v){
            if($i==$hi){
                break;
            }
        }
        while($v<$data[--$j]){
            if($j==$lo){
                break;
            }
        }
        if($i>=$j){
            break;
        }
        self::exch($data,$i,$j);
    }
    self::each($data,$lo,$j);
    $k=$j;
    
    //
    quick($data,$lo,$k-1);
    quick($data,$k+1,$hi);
}
```

```php
static function exch(&$data,$i,$j){
    $tmp=$data[$i];
    $data[$i]=$data[$j];
    $data[$j]=$tmp;
}   
```