---
title: jquery+php不刷新删除table的tr
id: 609
categories:
  - PHP
date: 2012-02-07 16:01:23
tags:
---

js代码：

&lt;script language="javascript" src="js/jquery.js"&gt;&lt;/script&gt;
&lt;script language="javascript"&gt;
$().ready(function(){
$("a.del").click(function(){
if(confirm("确定删除?"))
{
var id = $(this).attr("value");
var str="ac=del"+"&amp;id="+id;
$.ajax({
type:"post",
url:"jquery_del_trAct.php",
data:str,
dataType:"html",
success:function(data){
if(data == 1){
alert("删除成功!");
}else{
alert("操作失败！");
return false;
}
}
}); &lt;!--more--&gt;
$(this).parent("td").parent("tr").remove();
}
else
{
return false;
}
});

$("#msg").ajaxStart(function(){
$(this).html("正在加载。。。。");
});<!--more-->

$("#msg").ajaxSuccess(function(){
$(this).html("加载完成！");
});
});

&lt;/script&gt;

html和php混合代码：

&lt;?php
require_once('config.inc.php');
require_once('include/dbConnection.class.php');
$db = new dbConnection(DSN);
$query = "select * from member";
$db -&gt; execute($query);
$result = $db -&gt; fetch();
?&gt;

&lt;div style="height:100px; width:200px;" id="msg"&gt;&lt;/div&gt;
&lt;table width="400" border="0" align="center" cellpadding="0" cellspacing="1" bgcolor="#FFFFFF"&gt;
&lt;?php
foreach($result as $v){
?&gt;
&lt;tr&gt;
&lt;td width="300" height="25" align="left" bgcolor="#EEEEEE"&gt;&lt;?php echo $v['userName'];?&gt;&lt;/td&gt;
&lt;td width="100" align="center" bgcolor="#EEEEEE"&gt;&lt;a value="&lt;?php echo $v['id'];?&gt;" href="javascript:;"&gt;删除&lt;/a&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;?php
}
?&gt;
&lt;/table&gt;

php处理代码：

&lt;?php
require_once('config.inc.php');
require_once('include/dbConnection.class.php');
$db = new dbConnection(DSN);
$ac = $_REQUEST['ac'];
if($ac == "del"){
$id = intval($_POST['id']);
$query = "delete from member where id ={$id}";
$db -&gt; execute($query,2);
if($db -&gt;affectedRows &gt; 0){
echo 1;
}else{
echo 0;
}
}
?&gt;

注意：php代码都是按照我自己管用的写法写的，大家就按照自己的写就对了！