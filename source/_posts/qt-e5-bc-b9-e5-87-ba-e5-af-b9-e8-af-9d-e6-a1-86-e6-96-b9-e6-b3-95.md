---
title: Qt 弹出对话框方法
tags:
  - Qt
id: 923
categories:
  - Qt
date: 2012-03-01 20:09:35
---

1：用Qtcreator创建一个工程命名为Widget,创建.ui文件并添加相关控件，以添加pushButton为例

2：新创建一个Dialog文件，在dialog.ui文件中创建对话框。

3：实现点击pushButton弹出dialog对话框，在widget工程下添加dialog.h,dialog.cpp,dialog.ui,ui_dialog.h，并在widget.h文件中添加#include “dialog.h”。

4：在widget.h文件中添加private slots： void  on_pushButton_clicked（）;    在private中声明： Dialog  *clickPushButton 。

5：在widget.cpp文件中添加如下代码：

     void  Widget：：on_pushButton_clicked（）    {
            clickPushButton  = new Dialog（this）；
            clickPushButton->show();
  }

实现结果：点击pushButton按钮，弹出Dialog对话框。