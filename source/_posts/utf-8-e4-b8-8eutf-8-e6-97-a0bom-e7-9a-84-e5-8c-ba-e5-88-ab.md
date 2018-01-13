---
title: utf-8与utf-8(无BOM)的区别
id: 1045
categories:
  - PHP
date: 2012-07-27 06:56:49
tags:
---

BOM——Byte Order Mark，就是字节序标记

在UCS 编码中有一个叫做"ZERO WIDTH NO-BREAK SPACE"的字符，它的编码是FEFF。而FFFE在UCS中是不存在的字符，所以不应该出现在实际传输中。UCS规范建议我们在传输字节流前，先传输字符"ZERO WIDTH NO-BREAK SPACE"。这样如果接收者收到FEFF，就表明这个字节流是Big-Endian的；如果收到FFFE，就表明这个字节流是Little-Endian的。因此字符"ZERO WIDTH NO-BREAK SPACE"又被称作BOM。

UTF-8不需要BOM来表明字节顺序，但可以用BOM来表明编码方式。字符"ZERO WIDTH NO-BREAK SPACE"的UTF-8编码是EF BB BF。所以如果接收者收到以EF BB BF开头的字节流，就知道这是UTF-8编码了。

UTF-8编码的文件中，BOM占三个字节。如果用记事本把一个文本文件另存为UTF-8编码方式的话，用UE打开这个文件，切换到十六进制编辑状态就可以看到开头的FFFE了。这是个标识UTF-8编码文件的好办法，软件通过BOM来识别这个文件是否是UTF-8编码，很多软件还要求读入的文件必须带BOM。可是，还是有很多软件不能识别BOM。

在Firefox早期的版本里，扩展是不能有BOM的，不过Firefox 1.5以后的版本已经开始支持BOM了。现在又发现，PHP也不支持BOM。PHP在设计时就没有考虑BOM的问题，也就是说他不会忽略UTF-8编码的文件开头BOM的那三个字符。<!--more-->

由于必须在在Bo-Blog的wiki看到，同样使用PHP的Bo-Blog也一样受到BOM的困扰。其中有提到另一个麻烦：“受COOKIE送出机制的限制，在这些文件开头已经有BOM的文件中，COOKIE无法送出（因为在COOKIE送出前PHP已经送出了文件头），所以登入和登出功能失效。一切依赖COOKIE、SESSION实现的功能全部无效。”这个应该就是Wordpress后台出现空白页面的原因了，因为任何一个被执行的文件包含了BOM，这三个字符都将被送出，导致依赖cookies和session的功能失效。

解决的办法嘛，如果只包含英文字符(或者说ASCII编码内的字符)，就把文件存成ASCII码方式吧。用UE等编辑器的话，点文件-&gt;转换-&gt;UTF-8转ASCII，或者在另存为里选择ASCII编码。如果是DOS格式的行尾符，可以用记事本打开，点另存为，选ASCII编码。如果包含中文字符的话，可以用UE的另存为功能，选择“UTF-8 无 BOM”即可。

utf-8本来就不应该加bom，除了让编辑器知道它是个utf-8之外什么用处都没有。实际上编辑器完全有能力在不太多的几个编码格式之间根据特征来判断一个文件是什么编码，就算不能自动识别，编辑器也应该有设置编码的地方。所以我觉得BOM对于utf-8来说是多余的东西。

utf-16才需要加bom。因为它是按unicode顺序编码，在BMP范围内是二字节，需要识别是大或小字节序。

实际上，我觉得utf-8引入大小字节序的概念太愚蠢了，不知道那些标准委员会是怎么想的。大小字节序存在的意义，在于cpu的处理方式。如果cpu是大字节序处理，那么对于小字节序，就必须做一层转换，这带来了效率上的下降。但是在实际应用里，谁会去关心大小字节序？文本编码引起字节序的概念，只能说那些制定标准的人太死板了。对于utf-16，我认为只要全世界都遵循一种字节序方式，那就没什么必要用BOM来标注了。

话说回来，PHP是不支持utf-16编码的文件的。因为例如$这个符号，在utf-8里也是两个字节，PHP解码器无法解析的。不知道PHP6内部处理引入unicode 的概念之后，对这个是否会有支持。

编码问题是一个说起来简单，但是实际上很繁琐的东西。很多程序，都有分层编码的概念。像MySQL，就分为client-&gt;connection-&gt;storage和storage-&gt;connection-&gt;result这些概念。storage又分为system,database,table,column。我有时候在想，有必要搞这么复杂嘛，TNND。像MySQL，谁用利用它这些特性阿？除非允许两个client在不同的编码环境下运作，否则它把client编码分离出来根本没有什么必要。大多数情况下，直接binary in/binary out就好了