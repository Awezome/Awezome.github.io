---
title: jquery validate验证插件，在ajax提交方式下的验证
id: 1117
categories:
  - Html
date: 2013-02-18 10:32:52
tags:
---

正常的表单都是使用submit按钮来提交，jquery  validate插件可以方便的做表单验证。

要做一个发送短信的功能，向目标表插入多条记录，界面采用ajax来提交表单，等待效果直接用ext的遮罩了。

但是如何验证却碰到问题。

解决方式很简单，表单跟正常表单一样，validate的submitHandler，invalidHandler这2个方法都需要覆盖，都return false;这样表单就不会在点击按钮的时候提交了，表单验证跟正常验证起作用。submitHandler在return 之前写上我们的表单处理代码就ok了。

//表单验证
     $("#query").validate({
         onkeyup  : false,
         onclick  : false,
         onfocusout : false,
             rules : {
                 msg : {
                     required    : true,
                     maxlength    : 10
                 }
             },
             messages:{
                 msg:{
                     required    : '请输入短信内容!',
                     maxlength    : '长度超过10!'
                 }
             },
             showErrors : function(errorMap, errorList) {
                 var msg = "";
                 $.each(errorList, function(i,v){
                   msg += (v.message+"rn");
                 });
                 if(msg!="")
                 Ext.Msg.alert('表单',msg + fix);
             },
             invalidHandler : function(){
                 return false;
             },
             submitHandler : function(){
                 //表单的处理
                 Ext.Msg.confirm("确认", "是否确认发送?" + fix, function(button,text){
                    if(button == 'yes'){
                            loadMarsk.show();
                            $.ajax({
                                url:'<%=basePath %>promotionAction.do?method=group',
                                dataType:'json',
                                type:'post',
                                data:$('#query').serialize(),
                                error:function(){
                                    Ext.Msg.alert('错误','请求错误！' + fix);
                                    loadMarsk.hide();
                                },
                                success:function(data){
                                    Ext.Msg.alert('成功',data.msg + fix);
                                    loadMarsk.hide();
                                }
                            })
                    }
                 } );   //confirm
                 return false;//阻止表单提交
             }
         });