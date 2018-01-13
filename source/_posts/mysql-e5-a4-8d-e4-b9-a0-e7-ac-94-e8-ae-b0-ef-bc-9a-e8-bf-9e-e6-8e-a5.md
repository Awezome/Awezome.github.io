---
title: MySQL复习笔记：连接
tags:
  - MySQL
id: 2536
categories:
  - MySQL
date: 2015-05-15 17:51:09
---

SQL连接
基于 ANSI 标准的 SQL 列出了五种 JOIN 方式:
内连接(INNER JOIN)
全外连接(FULL OUTER JOIN)
左外连接(LEFT OUTER JOIN)
右外连接(RIGHT OUTER JOIN)
交叉连接(CROSS JOIN)
在特定的情况下, 一张表(基本表, 视图, 或连接表)可以和自身进行连接, 成为自连接(self-join)。

MySQL 内连接

连接产生的结果集，可以定义为首先对两张表做笛卡尔积(交叉连接)——将 A 中的每一行和 B 中的每一行组合，然后返回满足连接谓词的记录。实际上 SQL 产品会尽可能用其他方式去实现连接，笛卡尔积运算是非常没效率的。
程序要应该特别注意连接依据的列可能包含 NULL 值，NULL 值不与任何值匹配(甚至和它本身) -- 除非连接条件中显式地使用 IS NULL 或 IS NOT NULL 等谓词。
`select a.*,b.* from tb_person as a inner join tb_order as b; 
等价于
select * from tb_person a,tb_order b where a.personId=b.personId;`

相等链接
select * from course c inner join student_course sc on c.cid = sc.cid;
SQL 提供了一种可选的简短符号去表达相等连接，它使用 USING 关键字。
`select * from course c inner join student_course sc using(cid);`
USING 结构并不仅仅是语法糖，上面查询的结果和使用显式谓词得到的查询得到的结果是不同的。特别地，在 USING 部分列出的列(column)将以只出现一次，且名称无表名修饰。
<!--more-->

交叉连接
交叉连接(cross join)，又称笛卡尔连接(cartesian join)。笛卡尔（Descartes）乘积又叫直积。假设集合A={a,b}，集合B={0,1,2}，则两个集合的笛卡尔积为{(a,0),(a,1),(a,2),(b,0),(b,1), (b,2)}。可以扩展到多个集合的情况。把表A和表B的数据进行一个N*M的组合，即笛卡尔积。在开发过程中我们肯定是要过滤数据，所以这种很少用。
`显式的交叉连接实例：
select * from course c cross join student_course sc;
隐式的交叉连接实例：
select * from course,student_course;`
交叉连接不会应用任何谓词去过滤结果表中的记录。可以用 WHERE 语句进一步过滤结果集。

MySQL左外连接
那么结果表中将包含"左表"(即表 A)的所有记录, 即使那些记录在"右表" B 没有符合连接条件的匹配。这意味着左外连接会返回左表的所有记录和右表中匹配记录的组合(如果右表中无匹配记录, 来自于右表的所有列的值设为 NULL)。如果左表的一行在右表中存在多个匹配行。那么左表的行会复制和右表匹配行一样的数量, 并进行组合生成连接结果。

MySQL不支持全连接。
一些数据库系统(如 MySQL)并不直接支持全连接，但它们可以通过左右外连接的并集(参: union)来模拟实现。
`select * from course c left join student_course sc on c.cid = sc.cid
union
select * from course c right join student_course sc on c.cid = sc.cid
where c.cid is not null;`

MySQL如何执行连接查询

mysql中的关联（join）一词所包含的意义比一般意义上理解的更广泛。总的来说，mysql认为任何一个查询都是一次关联——并不仅仅是一个查询需要到两个表匹配才叫关联 ，所以在mysql中，每一个查询，每一个片段（包括子查询，甚至基于单表的select）都可能是关联。
当前mysql关联执行的策略很简单：mysql对任何关联都执行嵌套循环关联操作，即mysql先在一个表中循环取出单条数据，然后再嵌套循环到下一个表中寻找匹配的行，依次下去直到找到所有表中匹配的行为止。
左外连接-left outer join
`mysql> select tb1.col1,tb2.col2
    -> from tb1 left outer join tb2 using col3
    -> where tb1.col1 in (5,6);`
对应的伪代码如下：
`outer_iter = iterator over tb1 where col1 in (5,6)
outer_row  = outer_iter.next
while outer_row
    inner_iter = iterator over tb2 where col3 = outer_row.col3
    inner_row  = inner_iter.next
    if inner_row
        while inner_row
            output [outer_row.col1, inner_row.col2]
            inner_row = inner_iter.next
        end
    else
        output [outer_row.col1,NULL]
    end
    outer_row = outer_row.next
end`
`
select * from tb_order as a left join tb_person as b on a.personId=b.personId and b.orderId=1;
select * from tb_order as a left join tb_person as b on a.personId=b.personId where b.orderId=1;`
这两条返回数是不一样的，第一个是取右表时有b.orderID=1,下面那条是指最终的结果集是b.orderId=1

from http://my.oschina.net/xinxingegeya/blog/385721