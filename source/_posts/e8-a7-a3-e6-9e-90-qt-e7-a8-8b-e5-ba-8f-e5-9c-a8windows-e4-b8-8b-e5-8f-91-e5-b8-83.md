---
title: 解析 Qt 程序在Windows 下发布
tags:
  - Qt
id: 945
categories:
  - Qt
date: 2012-03-24 17:51:36
---

Qt 程序在Windows 下发布是本文要介绍的内容，不多说了，先来看内容，针对这个问题，其实 Qt 的 manual 中解释的已经比较清楚了。下面是我根据自己的理解和实验后写的东西，希望比Qt文档容易理解一点。

下面不涉及静态编译(静态编译可以看看这儿)，只包含动态编译（也就是Qt默认的情况），主要又分 mingw 和 msvc 两种情况：
Mingw

首先，我们需要生成 release 模式的可执行程序（不少同学抱怨，一个小小程序却需要100多M的动态库，就是因为用的debug）
qmake
mingw32-make release

而后将 可执行文件 与 需要的动态库放到同一个文件夹下，一般需要
myprogram.exe
mingwm10.dll
libgcc_s_dw2-1.dll
qtcore4.dll
qtgui4.dll

有同学抱怨，动态库拷过去以后，程序报错 无法定位程序输入点于动态链接库QtCore4.dll上 ，这一般是由于系统中装了多套Qt的运行库，而你拷贝的不是Qt安装目录下的库所导致的。比如，当安装的是Qt SDK时，很容易导致这个问题，因为里面的Qt是mingw编译的，但里面的QtCreator是msvc编译的，所以不少人不小心就吧QtCreator带的Qt运行库拷过来了。<!--more-->

如果你不需要其他的插件，那么就可以发布程序了，然而不少同学抱怨 jpg、gif、bmp 等格式的图片无法显示，这是因为 Qt 原生支持 png，而其他格式需要通过插件支持(插件在 %QTDIR%/pluginsimageformats 目录下)

你只需将需要的插件拷贝到可执行程序所在的目录下的 imageformats 目录下即可
myprogram.exe
imageformatsqjpeg4.dll
imageformatsqgif4.dll

同样，如果你的程序需要gb2312、gbk编码支持，那么需要将 %QTDIR%pluginscodecs 目录下的相应插件拷贝到可执行程序所在目录下的 codecs 目录下
myprogram.exe
codecsqcncodecs4.dll

建议：不妨多看看Qt安装目录下的plugins目录，熟悉这些插件分别是做什么的，你发布的程序需要哪些。

现在，程序可以发布了。你现在也可以通过 nsis 来制作一个安装包。
msvc

如果用的VS2008 而不是mingw，发布的过程其实基本是一样的。

首先生成 release 模式的 可执行文件
qmake
nmake release

而后准备需要的动态库与插件
myprogram.exe
qtcore4.dll
qtgui4.dll
imageformats*4.dll

  因为是vc编译的，所以不需要mingw的 mingwm10.dll libgcc_s_dw2-1.dll ，取代他们的是VC2008的CC++ 运行库：
MSVCR90.DLL
MSVCP90.DLL

如果你用的Windows xp 之前的系统，那么只要将这两个运行库和可执行程序放于同一个目录即可。

但对于Windows xp (包括)之后的系统，这样做并不会正常工作，程序会报告：由于应用程序配置不正确，应用程序未能启动。这个问题有点复杂，其实解决方法很简单，只需在用户机器上安装1M多的VS2008可再发行包 vcredist_x86.exe 即可

该包会将运行库安装到 window系统目录下的 WinSxS 目录下，对xp之前的系统，还会将运行库同时安装到path路径下的目录内。其实如果用户装过其他人编写的VC2008的程序，机器上很应该已经装过该包了。
 或许你要问，如果不想安装 可再发行包怎么办，比如就想把需要dll一块和程序打包，我们可以这么做：

将文件夹 (如果你用的VS2008 express，该文件夹不存在)
<visual Studio Install Path>VCredist<architecture>Microsoft.VC90.CRT

直接复制到可执行程序所在目录
myprogram.exe
Microsoft.VC90.CRT*

注意：

（1）如果用户机器上已经安装了可再发行包，程序将永远不会使用Microsoft.VC90.CRT下的库。

（2）当采用这种方法时，如果同时发布插件（包括图片插件等），那么插件编译时必须：
CONFIG-=embed_manifest_dll

使得生成的插件中不嵌入manifest文件，否则插件不被程序识别(其实也可以识别，只要将 Microsoft.VC90.CRT 拷贝一份和插件放到同一文件夹即可，当然这种方式很不好，如果插件分布在几个目录下，要放置Microsoft.VC90.CRT的很多副本)。

工具

1\. 一定要记住： Dependency Walker 是你的好帮手，它会告诉你你的 exe 和 dll需要哪些库，以及它加载的动态库都在哪个文件夹内 等

2\. 最好准备一个进程查看的工具，比如微软的 Process Explorer等，来查看你的程序到底加载了哪些动态库(加载了哪些插件等)

小结：关于解析 Qt 程序在Windows 下发布的内容介绍完了，希望本文对你有所帮助。