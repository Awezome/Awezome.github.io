---
title: QSqlQuery---Qt
tags:
  - Qt
id: 970
categories:
  - Qt
date: 2012-04-04 08:31:50
---

QSqlQuery提供了对数据库记录的Select、Insert、Update、Delete操作。

SELECT操作：

QSqlQuery query;

query.exec("SELECT name, salary FROM employee WHERE salary > 50000");
while (query.next()) {
    QString name = query.value(0).toString();
    int salary = query.value(1).toInt();
    qDebug() << name << salary;
}

通过QSqlQuery::next()、QSqlQuery::previous()、QSqlQuery::first()、QSqlQuery::last()、QSqlQuery::seek()，可以得到下一条、上一条、第一条、最后一条、任意一条记录的位置。<!--more-->

INSERT操作：

//单一插入数据

QSqlQuery query;

query.prepare("INSERT INTO employee (id, name, salary) "

                        "VALUES (:id, :name, :salary)");

query.bindValue(":id", 1001);

query.bindValue(":name", "Thad Beaumont");

query.bindValue(":salary", 65000);

query.exec();

//批量插入数据

QSqlQuery query;

query.prepare("insert into myTable values (?, ?)");

QVariantList ints;

ints << 1 << 2 << 3 << 4;

query.addBindValue(ints);

QVariantList names;

names << "Harald" << "Boris" << "Trond" << QVariant(QVariant::String);

query.addBindValue(names);

if (!query.execBatch())

    qDebug() << query.lastError();

UPDATE操作：

QSqlQuery query;

query.prepare("UPDATE employee SET salary = ? WHERE id = 1003");

query.bindValue(0, 70000);

query.exe();

DELETE操作：

QSqlQuery query;

query.exec("DELETE FROM employee WHERE id = 1007");

事务处理：

QSqlDatabase::database().transaction();

QSqlQuery query;

query.exec("SELECT id FROM employee WHERE name = 'Torild Halvorsen'");

if (query.next()) {

    int employeeId = query.value(0).toInt();

    query.exec("INSERT INTO project (id, name, ownerid) "

                       "VALUES (201, 'Manhattan Project', "

                       + QString::number(employeeId) + ")");

}

QSqlDatabase::database().commit();

如果数据库引擎支持事务处理，则函数QSqlDriver::hasFeature(QSqlDriver::Transactions)将返回 真。

可以通过调用QSqlDatabase::transaction()来初始化一个事务处理。之后执行你想在该事务处理的工作。

完了再执行QSqlDatabase::commit()来提交事务处理或QSqlDatabase::rollback()取消事务处理。

这里在举个QSqlDriver::hasFeature(QSqlDriver::QuerySize)例子，可以较快的统计查询记录行数。

QSqlQuery query;

int numRows;

query.exec("SELECT name, salary FROM employee WHERE salary > 50000");

QSqlDatabase defaultDB = QSqlDatabase::database();

if (defaultDB.driver()->hasFeature(QSqlDriver::QuerySize)) {

    numRows = query.size();

} else {

     // this can be very slow

     query.last();

     numRows = query.at() + 1;

}

存储过程：

AsciiToInt()是数据库中的一个存储过程。

但我在网上以前好像看过说是SQL Server中的存储过程是通过"EXEC"完成的，而不是"CALL"，这里我不确定！留下一个疑问吧~

QSqlQuery query;

query.prepare("CALL AsciiToInt(?, ?)");

query.bindValue(0, "A");

query.bindValue(1, 0, QSql::Out);

query.exec();

int i = query.boundValue(1).toInt(); // i is 65

■、使用SQL Model类

QSqlQueryModel：一个只读的读取数据库数据的模型。

QSqlTableModel：一个可读写的单一表格模型，可以不用写SQL语句。

QSqlRelationalTableModel：QSqlTableModel的一个子类，可多表关联在一起。

这些类都继承于QAbstractTableModel，而它们又都继承于QAbstractItemModel。

QSqlQueryModel 只读模式，基于SQL查询基础。

QSqlQueryModel model;

model.setQuery("SELECT * FROM employee");

for (int i = 0; i < model.rowCount(); ++i) {

    int id = model.record(i).value("id").toInt();

    QString name = model.record(i).value("name").toString();

    qDebug() << id << name;

}

QSqlTableModel 可对单一表操作，进行读写操作。

//读取数据

QSqlTableModel model;

model.setTable("employee");

model.setFilter("salary > 50000");

model.setSort(2, Qt::DescendingOrder);

model.select();

for (int i = 0; i < model.rowCount(); ++i) {

    QString name = model.record(i).value("name").toString();

    int salary = model.record(i).value("salary").toInt();

    qDebug() << name << salary;

}

//通过QSqlTableModel::setRecord()修改数据

for (int i = 0; i < model.rowCount(); ++i) {

    QSqlRecord record = model.record(i);

    double salary = record.value("salary").toInt();

    salary *= 1.1;

    record.setValue("salary", salary);

    model.setRecord(i, record);

}

model.submitAll();

//通过QSqlTableModel::setData()来update一条记录

model.setData(model.index(row, column), 75000);

model.submitAll();

//insert一条记录

model.insertRows(row, 1);

model.setData(model.index(row, 0), 1013);

model.setData(model.index(row, 1), "Peter Gordon");

model.setData(model.index(row, 2), 68500);

model.submitAll();

//delete一条记录

model.removeRows(row, 5);

model.submitAll();

函数QSqlTableModel::submitAll()确保记录写入数据库中。

QSqlRelationalTableModel 通过外键实现了多表关联。

//employee表中关联city表、country表。

model->setTable("employee");

model->setRelation(2, QSqlRelation("city", "id", "name"));

model->setRelation(3, QSqlRelation("country", "id", "name"));

■、数据呈现视图中

QSqlQueryModel、QSqlTableModel、QSqlRelationalTableModel一般都是借助QListView、QTableView、QTreeView吧数据呈现出来的~

QSqlRelationalTableModel model;

model->setTable("employee");

model->setRelation(2, QSqlRelation("city", "id", "name"));

model->setRelation(3, QSqlRelation("country", "id", "name"));

//设置标题头部标签信息

model->setHeaderData(0, Qt::Horizontal, QObject::tr("ID"));

model->setHeaderData(1, Qt::Horizontal, QObject::tr("Name"));

model->setHeaderData(2, Qt::Horizontal, QObject::tr("City"));

model->setHeaderData(3, Qt::Horizontal, QObject::tr("Country"));

//值得注意的是，在查询时应该明确指明那个表的数据信息，以下两种方式是等价的。

model.setFilter(tr("city.name = '%1'").arg("Mucich"));

//model.setFilter(tr("employee.cityid = %1").arg(312));

model.select();

//借助QTableView，把数据信息显示出来,

QTableView *view = new QTableView;

view->setModel(model);

//将表中的项，设计为不能编辑模式

view->setEditTriggers(QAbstractItemView::NoEditTriggers);

view->show();

在讲一种通过QSqlField进行Insert、Update、Delete的操作。上边的例子，继续~

QSqlField idField("id", QVariant::Int);

QSqlField nameField("name", QVariant::String);

QSqlField cityIdField("cityId", QVariant::Int);

QSqlField countryIdField("countryId", QVariant::Int);

//一条记录 Id = 12、Name = vic.MINg、City = ShenYang、Country = China。(沈阳区号024、中国086)

idField.setValue(12);

nameField.setValue("vic.MINg");

cityIdField.setValue(24);

countryIdField.setValue(86);

//insert一条记录，-1表示在最尾端加入

QSqlRecord record;

record.append(idField);

record.append(nameField);

record.append(cityIdField);

record.append(countryIdField);

model->insertRecord(-1, record);

//update一条记录, row表示要修改的行

QSqlRecord record = model->record(row);

record.replace(1, nameField);

record.replace(2, cityIdField);

record.replace(3, countryIdField);

model->setRecord(row, record);

//delete一条记录, row表示要修改的行

model->removeRow(row);

■、数据呈现窗体中

通过QDataWidgetMapper可以在窗体控件与数据库中的记录关联在一起。

QDataWidgetMapper *mapper = new QDataWidgetMapper;

mapper->setModel(model);

mapper->addMapping(idSpinBox, 0);

mapper->addMapping(nameLineEdit, 1);

mapper->addMapping(cityComboBox, 2);

mapper->addMapping(countryComboBox, 3);

//可以通过toFirst()、toNext()、toPrevious()、toLast()、setCurrentIndex()来设置当前记录位置，显示相应数据

mapper->toFirst();

//信号、槽的机制model、view、mapper三个联系再一起

connect(view->selectionModel(), SIGNAL(currentRowChanged(QModelIndex,QModelIndex)),

        mapper, SLOT(setCurrentModelIndex(QModelIndex)));