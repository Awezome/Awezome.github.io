---
title: php post 接收数组
id: 1081
categories:
  - PHP
date: 2012-09-29 21:47:27
tags:
---

第一中方法
&lt;form action="test1.php" method="post"&gt;
&lt;?

for($i=0;$i&lt;10;$i++){
?&gt;
&lt;input type="checkbox" name="interests[]（不能去掉[]）" value="&lt;?=$i?&gt;"&gt;test&lt;?=$i?&gt;&lt;br&gt;
&lt;?
}

?&gt;
&lt;input type="submit"&gt;

&lt;/form&gt;

&nbsp;

test1.php

&nbsp;

&lt;?php

foreach($_POST as $key =&gt; $val){
if(is_array($val)){
foreach($val as $v2){
echo "$v2&lt;br&gt;";
}
}
}

?&gt;

&nbsp;

第二种用法

&nbsp;

test3.php

&nbsp;

&lt;?php

&nbsp;

if(isset($_POST['submit'])){

$users = $_POST['user'];

foreach($users as $key=&gt;$val){

echo 'user ',$key,' = ',$val,'&lt;br /&gt;';

}

}

?&gt;

&lt;form method="post"&gt;

zhangsan &lt;input type="text" name="user[zhangsan]" value="0" /&gt;&lt;br /&gt;

lisi &lt;input type="text" name="user[lisi]" value="1" /&gt;&lt;br /&gt;

wangwu &lt;input type="text" name="user[wangwu]" value="2" /&gt;&lt;br /&gt;

zhaoliu &lt;input type="text" name="user[zhaoliu]" value="3" /&gt;&lt;br /&gt;

&lt;input type="submit" name="submit" value="提交" /&gt;

&lt;/form&gt;

1