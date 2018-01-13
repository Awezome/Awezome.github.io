---
title: 在ui->widget上绘制带坐标轴的图像
id: 973
categories:
  - Qt
date: 2012-04-07 08:05:15
tags:
---

=====================================Widget.h=====================================

#ifndef WIDGET_H

#define WIDGET_H

#include <qwidget>

namespace Ui {

    class Widget;

}

class Widget : public QWidget {

    Q_OBJECT

public:

    Widget(QWidget *parent = 0);

    ~Widget();

protected:

    virtual void changeEvent(QEvent *e);

    virtual bool eventFilter(QObject *watched, QEvent *e);

    void paintOnWidget(QWidget *w);

private:

    Ui::Widget *ui;

};

#endif // WIDGET_H
<!--more-->=====================================Widget.cpp=====================================

#include "Widget.h"

#include "ui_Widget.h"

#include <qtGui/QPainter>

Widget::Widget(QWidget *parent) : QWidget(parent), ui(new Ui::Widget) {

    ui->setupUi(this);

    ui->widget->installEventFilter(this);

}

Widget::~Widget() {

    delete ui;

}

void Widget::changeEvent(QEvent *e) {

    QWidget::changeEvent(e);

    switch (e->type()) {

    case QEvent::LanguageChange:

        ui->retranslateUi(this);

        break;

    default:

        break;

    }

}

bool Widget::eventFilter(QObject *watched, QEvent *e) {

    if (watched == ui->widget) {

        if (e->type() == QEvent::Paint) {

            paintOnWidget(ui->widget);

            return true;

        }

    }

    return QWidget::eventFilter(watched, e);

}

void Widget::paintOnWidget(QWidget *w) {

    QPainter painter(w);

    QFontMetrics metrics = painter.fontMetrics();

    int textHeight = metrics.ascent() + metrics.descent();

    int leftWidth = metrics.width(tr("9000")) + 5;

    int rightWidth = metrics.width(tr("(日)"));

    int width = w->size().width() - leftWidth - rightWidth;

    int height = w->size().height() - 3 * textHeight;

    // 绘制外框

    painter.drawRect(0, 0, w->size().width() -1, w->size().height() - 1);

    //　移动坐标系

    //painter.translate(inset * 2, ui->yearWidget->size().height() - inset);

    painter.translate(leftWidth, 1.75 * textHeight + height);

    int totalCount = 9000; // 默认每年收入9000件衣服

    int count = 10;        // 分成10成

    float deltaX = width / 12.0f;         // x坐标上每分的宽度

    float deltaY = (float)height / count; // y坐标上每分的宽度

    // 画横坐标

    painter.drawLine(0, 0, width, 0);

    for (int i = 1; i <= 12; ++i) {

        QString month = tr("%1月").arg(i);

        int stringWidth = metrics.width(month);

        // 绘制坐标刻度

        painter.drawLine(deltaX * i, 0, deltaX * i, 4);

        // 绘制坐标处的月

        int monthX = deltaX * (i - 1) + ((deltaX - stringWidth) / 2);

        painter.drawText(monthX, textHeight, month);

    }

    // 画纵坐标

    painter.drawLine(0, 0, 0, -height);

    painter.drawText(-metrics.width(tr("(件)")),

                     -(deltaY * count + textHeight / 2 + metrics.descent()),

                     tr("(件)"));

    for (int i = 1; i <= count; ++i) {

        QString value = QString("%1").arg(i * totalCount / count);

        int stringWidth = metrics.width(value);

        // 绘制坐标刻度

        painter.drawLine(-4, -i * deltaY, 0, -i * deltaY);

        // 绘制坐标值

        //painter.drawText(-stringWidth - 4, -i * deltaY + stringHeight / 2, value);

        painter.drawText(-stringWidth - 4, -(deltaY * i + textHeight / 2 - metrics.ascent()), value);

    }

    //    // 绘制每个月收到的服饰

    //    painter.setBrush(Qt::BDiagPattern);

    //    for (int i = 0; i < yearList.size(); ++i) {

    //        painter.setPen(Qt::black);

    //        int fineryCount = yearList.at(i); // 第i + 1个月收到的服饰件数

    //        int h = fineryCount / (float)totalCount * height;

    //        painter.drawRect(deltaX * i + 2, 0, deltaX - 4, -h);

    //

    //        // 绘制收到的服饰件数

    //        QString fineryString = QString("%1").arg(fineryCount);

    //        int stringWidth = metrics.width(fineryString);

    //

    //        if (h > height) {

    //            h = height;

    //        }

    //

    //        painter.setPen(Qt::red);

    //        //painter.drawText(deltaX * i + (deltaX - stringWidth) / 2, -(h + metrics.descent()), fineryString);

    //    }

}