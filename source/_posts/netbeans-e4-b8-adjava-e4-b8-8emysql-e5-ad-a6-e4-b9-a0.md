---
title: netbeans中java与mysql学习
id: 1085
categories:
  - Java
date: 2012-10-11 22:13:39
tags:
---

转载请注明出处：http://hi.baidu.com/520huan_byaxiom
axiom原创
一、下载mysql（for vwindows）http://dev.mysql.com/downloads/mysql/5.0.html#win32
版本信息：
+---------------------+
| version() |
+---------------------+
| 5.0.45-community-nt |
+---------------------+
二、安装mysql（人性化的安装，记得设密码，本文中用axiom）
三、检测mysql
开始-&gt;程序-&gt;MYsql-&gt;mysql server 5.0-&gt;mysql command line cilent 输入刚才设置密码（如果刚才你没有经过这步，就是没有密码咯，直接回车）
此时出现shell：mysql&gt;
mysql&gt;select version(); //键入select version();出现上面的版本信息

四、建立一个数据库axiom，并在中建立以表table,在其中加入一点数据；
mysql&gt;create database axiom;
mysql&gt;use axiom;
mysql&gt;create table mytable (name varchar(20),sex char(1),age int(4));
mysql&gt;insert into mytable values('axiom','m',20);
mysql&gt;insert into mytable values('huan','f',20);
mysql&gt;select * from mytable; //出现你所加入是数据
+-----------+------+------+
| name | sex | age |
+-----------+------+------+
| axiom | m | 20 |
| huan | f | 20 |
五、由于时间问题，你的netbeans就自己装咯！
六、下载jdbc mysql驱动在左边选择5.0版本，然后再又边选择for Windows
解压到盘中，然后在你新建的netbeans项目中的libraries中点右键add jre/folder 加入刚才解压的文件下面的mysql-connector-java-5.0.8-bin.jar和debug/mysql-connector-java-5.0.8-bin-g.jar
七、编写java代码，你也可以直接复制<!--more-->
/*
* To change this template, choose Tools | Templates
* and open the template in the editor.
*/

package com.axiom;

/**
*
* @author axiomhuan*/
import java.sql.*;
public class sqlConn
{
public static void main(String[] args)
{
try{
System.out.println("MySQL connection test");
Connection con = null;
Statement stat = null;
String urlstr = "jdbc:mysql://127.0.0.1:3306/axiom";
String user = "root";
String password = "axiom";
Class.forName("com.mysql.jdbc.Driver");
con = DriverManager.getConnection(urlstr, user, password);
stat = con.createStatement();
//stat.executeUpdate("insert into mytable values('axiomhuan','m',20)");
ResultSet rs = stat.executeQuery("select * from mytable");
while (rs.next()){
System.out.println("name:"+rs.getString("name"));
String st=rs.getString("sex");
char c = st.charAt(0); //为了得到sex中值判断性别是famale还是male
if(c=='f'){
System.out.println("sex: famile");
}else{
System.out.println("sex: mile");
}
System.out.println("age: "+rs.getInt("age"));

}
rs.close();
System.out.println("Test ends");
}catch(Exception e)
{
e.printStackTrace();
}}
}
单击右键run file
搞定！～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～