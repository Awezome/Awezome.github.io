---
title: Qt 出现“undefined reference to `vtable for”原因总结
id: 1176
categories:
  - Qt
date: 2014-01-22 11:03:52
tags:
---

由于Qt本身实现的机制所限，我们在使用Qt制作某些软件程序的时候，会遇到各种各样这样那样的问题，而且很多是很难，或者根本找不到原因的，即使解决了问题，如果有人问你为什么，你只能回答－－不知道。

今天我在这里列举的问题也是再编写Qt程序时，总是遇到的问题，问题普遍，而答案却不唯一，解释这一个问题的说法很多，往往只适合某一种情况，因为这个错误太笼统了，它就是－－ “undefined reference to `vtable for”可能你看着很熟悉，似乎在c++程序中也遇到过这个问题，你说对了，有时候这个错误，不只是qt的原因，还有你的c++程序的原因。

下面搜集了网上的一些出现的现象，对应解决方法，原因，基本上我都验证过，收录于此，以做备份。
一：预编译器打开宏Q_OBJECT，声明若干个由moc处理(implement)的成员函数。

如果得到类似于“undefined reference to vtable for LcdNumber”的编译错误(if you get compiler errors along the lines of "undefined reference to vtable for LcdNumber")，你可能是忘记了执行moc，或者忘记了将moc输出加入到link命令里。

某一个类中如果加入Q_OBJECT后,则link时提示:undefined reference to vtable for "xxx::xxx".删掉它则没有任何问题.<!--more-->

解决:尝试(1):把所有的obj文件和uic文件删除,重新编译.仍然失败.

去trolltech的mail lists找到原因: 因为qmake生成Makefile的时候,这个类的头文件中并没有Q_OBJECT,所以在相应的Makefile里面并没有用moc xxx.h命令,最终导致链接失败.重新运行qmake,问题解决.
在查找解决方法的时候,附带发现一点:
qmake 不会处理.cpp文件里的Q_OBJECT,所以,如果在.cpp文件中有它的话,也会产生undefined reference to vtable for "xxx::xxx". 这时,需要先用moc xxxx.cpp生成相应的moc文件,再包含到.cpp里面去,才能解决这个问题.

这里可以发现问题的出现是因为没有moc生成相应的moc文件，之后连接就出问题。
我找了好多源码之类的问题，就是没有找pro的错误，后来想到qt中moc我们是有make做的
qt的make编译是根据Makefile来的，而Makefile是由pro文件来的。这才想到了找pro文件的错误。
from: http://www.cublog.cn/u/16292/showart_136087.html

二：undefined reference to vtable for "xxx::xxx"

今天碰到了这个问题，终于被我google到了：
http://www.cublog.cn/opera/showart.php?blogid=8650&amp;id=49526
原 因：qmake不会处理.cpp文件里的Q_OBJECT,所以,如果在.cpp文件中有它的话,也会产生undefined reference to vtable for "xxx::xxx". 这时,需要先用moc xxxx.cpp生成相应的moc文件,再包含到.cpp里面去,才能解决这个问题.

其他：
1.问题: QGLViewer中的函数不能正常link.

解决: 翻看其源代码,发现是因为从源码安装libQGLViewer时,编译用了Qt 3,而我的程序中用Qt4 编译.所以必须重新用Qt4编译.但是,更改QTDIR 环境变量为Qt4后,重新编译的话,qmake生成makefile时就提示出错.进一步发现,是因为虽然设了QTDIR为Qt4,头文件和库文件都会使 用Qt4,但是moc,uic等都是用的qt3版的,再把PATH环境变量改动后,一切ok.

2.问题:某一个类中如果加入Q_OBJECT后, 则link时提示:undefined reference to vtable for "xxx::xxx".删掉它则没有任何问题.

解决:尝试(1):把所有的obj文件和uic文件删除,重新编译.仍然失败.去trolltech的 mail lists找到原因: 因为qmake生成Makefile的时候,这个类的头文件中并没有Q_OBJECT,所以在相应的Makefile里面并没有用moc xxx.h命令,最终导致链接失败.重新运行qmake,问题解决.在查找解决方法的时候,附带发现一点:

qmake 不会处理.cpp文件里的Q_OBJECT,所以,如果在.cpp文件中有它的话,也会产生undefined reference to vtable for "xxx::xxx". 这时,需要先用moc xxxx.cpp生成相应的moc文件,再包含到.cpp里面去,才能解决这个问题.

3\. 看Qt的reference发现: 可以connect(pObjA, SIGNAL(someSignalA()),pObjB,SIGNAL(someSignalB()));这样pObjA发出的someSingalA 会导致pObjB发出someSignalB,从而形成信号接力.

from：http://blog.donews.com/netexe/archive/2006/02/09/720544.aspx

三：编译报错的部分是：

CODE:

[Copy to clipboard]
tmpobjrelease_sharedmyplot.o(.text+0x194):myplot.cpp: undefined reference to
`vtable for MyPlot'
tmpobjrelease_sharedmyplot.o(.text+0x19b):myplot.cpp: undefined reference to
`vtable for MyPlot'
tmpobjrelease_sharedmyplot.o(.text+0x934):myplot.cpp: undefined reference to
`vtable for MyPlot'
tmpobjrelease_sharedmyplot.o(.text+0x93b):myplot.cpp: undefined reference to
`vtable for MyPlot'
collect2: ld returned 1 exit status你 MyPlot 是不是声明了什么 virtual 的 方法， 但在实现（cpp）文件里没有实现。
看了一下， 现在推测一下。 MyPlot 继承了 QwtPlot， 现在报的这个错， 应该是这个意思，
1\. QwtPlot 里含有 vritual 方法， 所以你的继承类需要一个 virtual 的析构函数
2\. QwtPlot 里含有 纯虚的方法， 你没有继承， 而这个可能性比较小

所以， 你可以：
在MyPlot里声明一下一个虚的析构函数
virtual ~MyPlot() {}

我们来看一个简单些的例子吧：

#include
#include

class myWidget :public QWidget
{
Q_OBJECT
public:
myWidget(QWidget *parent = 0);
public slots:
void shutDown();
private:
QPushButton *exit;
};

myWidget::myWidget(QWidget *parent):QWidget(parent)
{
setMinimumSize(120, 180);
setMaximumSize(120, 180);
exit = new QPushButton("ShutDown!",this);
connect(exit,SIGNAL(clicked()),this,SLOT(shutDown()));
}

void myWidget::shutDown()
{
system("halt");
}

int main(int argc, char **argv)
{
QApplication app(argc, argv);
myWidget *w = new myWidget();
w-&gt;show();
app.exec();
}

还是报错：

shut.o(.text+0x21): In function `myWidget::myWidget[not-in-charge](QWidget*, char const*)':
: undefined reference to `vtable for myWidget'
shut.o(.text+0x28): In function `myWidget::myWidget[not-in-charge](QWidget*, char const*)':
: undefined reference to `vtable for myWidget'
shut.o(.text+0x141): In function `myWidget::myWidget[in-charge](QWidget*, char const*)':
: undefined reference to `vtable for myWidget'
shut.o(.text+0x148): In function `myWidget::myWidget[in-charge](QWidget*, char const*)':
: undefined reference to `vtable for myWidget'
collect2: ld returned 1 exit status这是我从l搜索引擎上搜到的例子，从代码中好像看不到明显的错误吧
我自己试了一下编译也是出错

刚才那个简单的例子的原因可能是这样的：

2.问题:某一个类中如果加入Q_OBJECT后,则link时提示:undefined reference to vtable for "xxx::xxx".删掉它则没有任何问题.

解决:尝试(1):把所有的obj文件和uic文件删除,重新编译.仍然失败.

去trolltech的mail lists找到原因: 因为qmake生成Makefile的时候,这个类的头文件中并没有Q_OBJECT,所以在相应的Makefile里面并没有用moc xxx.h命令,最终导致链接失败.重新运行qmake,问题解决.

在查找解决方法的时候,附带发现一点:

qmake不会处理.cpp文件里的Q_OBJECT,所以,如果在.cpp文件中有它的话,也会产生undefined reference to vtable for "xxx::xxx". 这时,需要先用moc xxxx.cpp生成相应的moc文件,再包含到.cpp里面去,才能解决这个问题.知道你的问题了， 上面的例子不好， 因为你这个把main和类放在一起了。 如果分开， 可能就没问题， 包括 h和 cpp 分开。

要解决上面的问题， 在你main.cpp 最后加一行
#include "main.moc"

然后把除了 main.cpp 的其它文件都删了。重新来， 没问题的。

所以， 我估计你前面提到的问题也是没有链接 moc 文件。 你在你的 myplot.cpp 文件最后加一行： #include "myplot.moc" 试试看。

另外： 你main 没有返回值， 编译会警告， 而且程序最后一行不是只含有回车， 也会警告。