---
title: Qt setWindowFlags的使用
tags:
  - Qt
id: 921
categories:
  - Qt
date: 2012-03-01 11:44:28
---

setWindowFlags(Qt::WindowCloseButtonHint);//哈哈窗口只有一个关闭按钮

使用方法：1： clientMainWindow::clientMainWindow(QWidget *parent) : QMainWindow(parent,Qt::WindowCloseButtonHint) { }
2
clientMainWindow::clientMainWindow(QWidget *parent)
: QMainWindow(parent ) {
setWindowFlags(Qt::WindowCloseButtonHint);
}
更多窗口样试：
Qt::WindowContextHelpButtonHint 像对话框一样，有个问号和关闭按钮
Qt::CustomizeWindowHint 标题栏也没有 按钮也没有 在那里出现就站在那里不到，也不能移动和拖到，任务栏右击什么也没有，任务栏窗口名也没有，做流氓软件很好，但是可惜可以从任务管理器里关闭 灰色
Qt::WindowTitleHint 也是窗口只有一个关闭按钮
Qt::WindowSystemMenuHint 还是一样只有一个关闭按钮<!--more-->
Qt::WindowCloseButtonHint 还是一样只有一个关闭按钮
Qt::WindowMaximizeButtonHint 一看就知道最小化按钮怎么了。。。原来不可用。。。。
Qt::WindowMinimizeButtonHint 还原按钮不可用。。
Qt::SubWindow 窗口没有按钮但是有标题栏 任务里什么也看不到
Qt::Desktop 没有显示在桌面也没在任务。但是任务管里器里还是有的。。。
Qt::SplashScreen 标题栏也没有 按钮也没有 在那里出现就站在那里不到，也不能移动和拖到，任务栏右击什么也没有，任务栏窗口名也没有， 但是可惜可以从任务管理器里关闭 白色

Qt::ToolTip 标题栏也没有 按钮也没有 在那里出现就站在那里不到，也不能移动和拖到，任务栏右击什么也没有，任务栏窗口名也没有， 但是可惜可以从任务管理器里关闭 白色 有个好外，顶层窗口 一直都是在最上面..

Qt::Tool 有一个小小的关闭按钮，但是好像不能真正的关闭。。。。