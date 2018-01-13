---
title: struts2验证失败后返回input，select中list数据丢失问题
id: 1122
categories:
  - Java
date: 2013-02-14 10:34:49
tags:
---

【转】很多人都在问，struts2验证失败后select中list数据丢失的问题
could not be resolved as a collection/array/map/enumeration/iterator type. Example: people or people.{name}。
 其实这个问题很简单，大多数人，是通过action跳转到数据编辑页面的，这样做的目的是方便把数据库里的数据，反显到页面。同时也可以通过该action 将select中的列表数据从数据库中取出，传递给页面。但是，这里出现一个问题，那就是在struts2验证失败后返回input的时候，页面不是通过该action的该方法传递回去的（实际上是通过DefaultWorkflowInterceptor拦截器拦截回去的），所以这个时候页面就找不到sleect的数据集合，从而报错。有的人也许会问，修改input的type为redirect，直接掉转到那个action上去，呵呵，这种方法虽然可以保证得到select的数据集合，因为redirect的性质，我们丢失了之前验证的错误信息。所以还是不能解决问题，那么有的人可能说了，那么改用chain就可以了吧，如果改用chain，我们知道，chain是action链的掉转，执行action还是要经过拦截器，所以因为你带有验证错误信息，你还是会被DefaultWorkflowInterceptor拦截器拦截，还是会报错，而且是chain链错。
     其实，可以反一种思路，既然走跳转不成功，我们可不可以在页面上下功夫呢。其实我们的select的list能不能不经过action而直接得到数据呢，是可以的，我们的ognl可以访问某个对象的某个方法，也可以访问某个静态类的静态属性，静态方法。所以我们可以将这些数据通过对象方法访问，也可以通过静态类的静态方法去访问，具体怎么做，根据需要写就可以了。代码就不在这里写了，因为非常简单。