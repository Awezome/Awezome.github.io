---
title: 关闭QMessageBox的时候，整个程序也退出
id: 1126
categories:
  - Qt
date: 2013-03-30 10:36:28
tags:
---

在程序中加入：QApplication::setQuitOnLastWindowClosed(false);

如：
QApplication::setQuitOnLastWindowClosed(false);QMessageBoxmessage(QMessageBox::NoIcon,"Show Qt","Do you want to show Qt dialog?",QMessageBox::Yes|QMessageBox::No);if(message.exec()==QMessageBox::Yes){delLoginFile();}else{qDebug()<<"~~~~~~~~~~~";return;}