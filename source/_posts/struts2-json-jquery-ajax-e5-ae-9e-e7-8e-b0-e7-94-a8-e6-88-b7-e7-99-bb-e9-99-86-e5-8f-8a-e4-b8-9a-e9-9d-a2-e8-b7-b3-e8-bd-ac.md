---
title: struts2 json jquery ajax实现用户登陆及业面跳转
id: 1119
categories:
  - Java
date: 2013-02-15 10:33:42
tags:
---

实现的功能是在登陆页面输入用户名

   错误用户名,页面返回错误信息,页面不刷新.

   输入正确的用户名(scott tiger),页面部刷新返回用户的详细信息,提示登陆成功,并且提示页面3秒钟后自动跳转到welcome.jsp页面。

   贴图太麻烦，这里直接贴代码了，方便大家copy,调试，呵呵。

1.配置strut2这里就不说了，网上很多。

  lib包如下：

  commons-beanutils-1.7.0.jar  commons-collections-3.2.jar    commons-lang-2.4.jar

  commons-logging-1.0.4.jar     commons-logging-api-1.1.jar   *ezmorph-1.0.4.jar

  freemarker-2.3.8.jar               *json-lib-2.3-jdk15.jar             * jsonplugin-0.32.jar

  ognl-2.6.11.jar                       struts2-core-2.0.14.jar             xwork-2.0.7.jar

  Jquery:jquery-1.3.2.js   放在目录: WebContent/js/jquery-1.3.2.js

2\. loginJqueryAjax.jsp 代码：(根WebContent/下)

 --------------------------------------------

   <%@ page language="java" contentType="text/html; charset=UTF-8"
   pageEncoding="UTF-8"%>
 <%@taglib prefix="s" uri="/struts-tags"%>
 <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
 <title>Login Ajax Page</title>
 <script type="text/javascript" src="js/jquery-1.3.2.js"></script>
 <script type="text/javascript" src="js/index.js"></script>
 <s:head theme="ajax" />
 </head>
 <body>
 <center><s:form>

   <s:textfield id="username" label="User Name" name="user.username" />
   <s:textfield id="password" label="User Password" name="user.password" />

   <input type="button" id="submit" value="login">
   <s:div id="msg"></s:div>

   <div id="quartz" style="display: none">3s end ,the page will
   redirect <span id="num">3</span>s</div>

 </s:form>

 <table border="0" width="360">
   <caption>Person information</caption>
   <tr>
     <td>userName:</td>
     <td>
     <div id="personname"></div>
     </td>
   </tr>

   <tr>
     <td>age:</td>
     <td>
     <div id="personage"></div>
     </td>
   </tr>

   <tr>
     <td>email:</td>
     <td>
     <div id="personemail"></div>
     </td>
   </tr>

   <tr>
     <td>birth:</td>
     <td>
     <div id="personbirth"></div>
     </td>
   </tr>
 </table>
 </center>
 </body>
 </html>

3\. index.js   /WebContent/js/index.js

------------------------------------------------

$(document).ready(function() {
     $("#submit").click(function() {
       var params = $("input").serialize();
         $.ajax( {
           // 后台处理程序
           url : "login.action",
           // 数据发送方式
           type : "post",
           // 接收数据格式
           dataType : "json",

           data : params,
           // 要传递的数据
           // 回传函数
           timeout : 20000,// 设置请求超时时间（毫秒）。

           success : function(data) { // 请求成功后回调函数。

             //3秒后自动跳转，下面是相对路径
             if (data.nextPageFlag == true) {
               $("#msg").css("color","green");
               $("#msg").html(data.result+" password:"+data.user.password+" age:"+data.user.age);
               $("#quartz").css("background","#cccccc");
               $("#quartz").show();

               $("#personname").html(data.user.username);
               $("#personage").html(data.user.age);
               $("#personemail").html(data.user.email);
               $("#personbirth").html(data.user.birth);

               function jump(count) {
                 window.setTimeout(function() {
                   count--;
                   if (count > 0) {
                     $('#num').attr('innerHTML', count);
                     jump(count);
                   } else {
                     location.href = "pages/welcome.jsp";
                   }
                 }, 1000);
               }
               jump(3);

             }else{
               $("#msg").css("color","red");
               $("#msg").html(data.result);
             }

           }
         });
       });
   });

4\. JsonAction.java 代码:

-------------------------------

package action;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

import net.sf.json.JSONObject;

import com.opensymphony.xwork2.Action;
import com.opensymphony.xwork2.ActionContext;
import model.User;

public class JsonAction implements Action {
   private User user;
   private String username;
   private String result;
   private boolean nextPageFlag;

   @SuppressWarnings("unchecked")
   @Override
   public String execute() throws Exception {
     // TODO Auto-generated method stub
     // System.out.println(user);
     JSONObject jo = JSONObject.fromObject(this.getUser());
     System.out.println(jo);

     ActionContext.getContext().getSession().put("username", user.getUsername());

     if ("scott".equals(user.getUsername()) && "tiger".equals("tiger")) {
       user = this.getUserDetail();
       JSONObject jouserdetail = JSONObject.fromObject(this.getUser());
       System.out.println(jouserdetail);
       result = "login success, login name: " + user.getUsername();
       ActionContext.getContext().getSession().put("person", user);
       nextPageFlag = true;
     } else {
       result = "username/password is wrong";
       nextPageFlag = false;
     }

     return SUCCESS;
   }

   public User getUser() {
     return user;
   }

   public void setUser(User user) {
     this.user = user;
   }

   public String getResult() {
     return result;
   }

   public void setResult(String result) {
     this.result = result;
   }

   public String getUsername() {
     return username;
   }

   public void setUsername(String username) {
     this.username = username;
   }

   public boolean isNextPageFlag() {
     return nextPageFlag;
   }

   public void setNextPageFlag(boolean nextPageFlag) {
     this.nextPageFlag = nextPageFlag;
   }

   public User getUserDetail() {
     User userDetail = new User();
     userDetail.setUsername("jack.tian");
     userDetail.setPassword("jack.tian");
     userDetail.setAge(25);

     SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
     Date date = null;
     try {
       date = sdf.parse("2010-01-01");
     } catch (ParseException e) {
       // TODO Auto-generated catch block
       e.printStackTrace();
     }
     userDetail.setBirth(date);

     userDetail.setEmail("XX@xx.com");
     return userDetail;
   }
 }

5.User.java

---------------------------

package model;

import java.util.Date;

public class User{
   /**
    *
    */
   private static final long serialVersionUID = 4861789569977171659L;
   private String username;
   private String password;
   private Integer age;
   private String email;
   private Date birth;

   public User(){}

   public User(String username, String password){
     this.username = username;
     this.password = password;
   }
  ....get set method.

 }

6.struts.xml

------------------------------

<struts>

   <package name="struts2_login" extends="json-default">

     <action name="login" class="action.JsonAction">
       <result type="json"></result>
     </action>

   </package>
 </struts>

7.welcom.jsp /pages/welcome.jsp

-------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
     pageEncoding="UTF-8"%>
 <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
 <title>welcome</title>
 </head>
 <body>

User:${sessionScope.username},
login successfully.

</body>
 </html>