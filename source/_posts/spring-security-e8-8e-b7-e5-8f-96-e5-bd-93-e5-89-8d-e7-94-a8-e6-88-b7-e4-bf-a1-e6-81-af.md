---
title: spring security 获取当前用户信息
id: 1124
categories:
  - Java
date: 2013-03-10 10:35:45
tags:
---

如果只是想从页面上显示当前登陆的用户名，可以直接使用Spring Security提供的taglib。

<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>
<div>username : <sec:authentication property="name"/></div>

如果想在程序中获得当前登陆用户对应的对象。

UserDetails userDetails = (UserDetails) SecurityContextHolder.getContext()
    .getAuthentication()
    .getPrincipal();

如果想获得当前登陆用户所拥有的所有权限。

GrantedAuthority[] authorities = userDetails.getAuthorities();