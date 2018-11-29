---
title: '深入浅出MySQL阅读笔记一:SQL基础'
categories:
  - 数据库
  - MySQL
  - 深入浅出MySQL阅读笔记
tags:
  - 数据库
  - MySQL
  - MySQL数据库
  - 关系型数据库
  - 深入浅出MySQL
  - SQL基础
date: 2018-11-29 15:41:13
---
# 深入浅出MySQL阅读笔记一:SQL基础
`mysql控制台`
```
mysql -uroot -proot
```
sql语句分类
- DDL（Data Definition Languages）语句：数据定义语言，这些语句定义了不同的数据段、
数据库、表、列、索引等数据库对象的定义。常用的语句关键字主要包括 create、drop、alter
等。
- DML（Data Manipulation Language）语句：数据操纵语句，用于添加、删除、更新和查
询数据库记录，并检查数据完整性，常用的语句关键字主要包括 insert、delete、udpate 和
select 等。
- DCL（Data Control Language）语句：数据控制语句，用于控制不同数据段直接的许可和
访问级别的语句。这些语句定义了数据库、表、字段、用户的访问权限和安全级别。主要的
语句关键字包括 grant、revoke 等。

**MYSQL version 5.5.8**

## DDL
`数据库`:
```sql
-- 查看数据库
SHOW DATABASES;

-- 创建数据库
CREATE DATABASE dbname;

-- 使用数据库
USE dbname;

-- 删除数据库
DROP DATABASE dbname;
```

`mysql自动创建的数据库`:
- information_schema：主要存储了系统中的一些数据库对象信息。比如用户表信息、列信
息、权限信息、字符集信息、分区信息等。
- cluster：存储了系统的集群信息。
- mysql：存储了系统的用户权限信息。
- test：系统自动创建的测试数据库，任何用户都可以使用。

`表`:
```sql
-- 查看表
SHOW TABLES;

-- 创建表
CREATE TABLE tablename (column_name_1 column_type_1 constraints，
column_name_2 column_type_2 constraints ， ……column_name_n column_type_n
constraints）;
-- e.g.
 create table emp(
ename varchar(10),
hiredate date,
sal decimal(10,2),
deptno int(2));

-- 查看表定义
DESC tablename;

-- 查看创建表的语句
SHOW CREATE TABLE tablename \G;
-- \G表示记录能够按照字段竖着排列

-- 删除表
DROP TABLE tablename;

/* 对于已经创建好的表，尤其是已经有大量数据的表，如果需要对表做一些结构上的改变，我
们可以先将表删除（drop），然后再按照新的表定义重建表。这样做没有问题，但是必然要
做一些额外的工作，比如数据的重新加载。而且，如果有服务在访问表，也会对服务产生影
响。
因此，在大多数情况下，表结构的更改一般都使用 alter table 语句，以下是一些常用的命令。 */

-- 修改表字段定义
ALTER TABLE tablename MODIFY [COLUMN] column_definition [FIRST | AFTER col_name];
-- e.g.
alter table emp modify ename varchar(20);
-- 可选项[FIRST | AFTER col_name]改变字段的order
-- e.g. ename放到最后
alter table emp modify ename varchar(20) after deptno;

-- 增加表字段
ALTER TABLE tablename ADD [COLUMN] column_definition [FIRST | AFTER col_name]
-- e.g.
alter table emp add column age int(3);

-- 删除表字段
ALTER TABLE tablename DROP [COLUMN] col_name
-- e.g.
alter table emp drop column age;

-- 字段改名
ALTER TABLE tablename CHANGE [COLUMN] old_col_name column_definition
[FIRST|AFTER col_name]
-- e.g.
alter table emp change age age1 int(4) ;

/* 注意：CHANGE/FIRST|AFTER COLUMN 这些关键字都属于 MySQL 在标准 SQL 上的扩展，在
其他数据库上不一定适用。*/

-- 表改名
ALTER TABLE tablename RENAME [TO] new_tablename
-- e.g.
aletr table emp rename emp1;
```
## DML
```sql
-- 插入记录
INSERT INTO tablename (field1,field2,……fieldn) VALUES(value1,value2,……valuesn);
-- e.g.
insert into emp (ename,hiredate,sal,deptno) values('zzx1','2000-01-01','2000',1);
-- 插入所有字段时,字段名可以省略,但是 values 后面的顺序应该和字段的排列顺序一致
insert into emp values('lisa','2003-02-01','3000',2);
-- 对于含可空字段、非空但是含有默认值的字段、自增字段，可以不用在 insert 后的字段列表
-- 里面出现
insert into emp (ename,sal) values('dony',1000);
-- 这样实际插入的hiredate字段是null
--可以一次插入多条记录
-- e.g.
 insert into dept values(5,'dept5'),(6,'dept6');

-- 更新记录
UPDATE tablename SET field1=value1，field2.=value2，……fieldn=valuen [WHERE CONDITION]
-- 可以同时更新多个表中数据
UPDATE t1,t2…tn set t1.field1=expr1,tn.fieldn=exprn [WHERE CONDITION]

-- 删除记录
DELETE FROM tablename [WHERE CONDITION]
-- 同时删除多表记录
DELETE t1,t2…tn FROM t1,t2…tn [WHERE CONDITION]
-- 不加 where 条件将会把表的所有记录删除，所以操作时一定要小心

--查询记录
SELECT * FROM tablename [WHERE CONDITION]
-- 查询不重复的记录
select distinct deptno from emp;
-- 排序
SELECT * FROM tablename [WHERE CONDITION] [ORDER BY field1 [DESC|ASC] ， field2
[DESC|ASC]，……fieldn [DESC|ASC]]
-- 限制
SELECT ……[LIMIT offset_start,row_count]
-- 基本上,limit 总是在查询语句的最后

--聚合
SELECT [field1,field2,……fieldn] fun_name
FROM tablename
[WHERE where_contition]
[GROUP BY field1,field2,……fieldn
[WITH ROLLUP]]
[HAVING where_contition]
/* fun_name 表示要做的聚合操作，也就是聚合函数，常用的有 sum（求和）、count(*)（记
录数）、max（最大值）、min（最小值）。
- GROUP BY 关键字表示要进行分类聚合的字段，比如要按照部门分类统计员工数量，部门
就应该写在 group by 后面。
- WITH ROLLUP 是可选语法，表明是否对分类聚合后的结果进行再汇总。
- HAVING 关键字表示对分类后的结果再进行条件的过滤。*/
/* 我们尽可能用 where 先过滤记录，这样因为结果
集减小，将对聚合的效率大大提高，最后再根据逻辑看是否用 having 进行再过滤 */
-- 统计各部门人数和总人数
 select deptno,count(1) from emp group by deptno with rollup;

-- 表连接
/* 当需要同时显示多个表中的字段时，就可以用表连接来实现这样的功能。
从大类上分，表连接分为内连接和外连接，它们之间的最主要区别是內连接仅选出两张表中
互相匹配的记录，而外连接会选出其他不匹配的记录。我们最常用的是内连接 */
-- 查询出所有雇员的名字和所在部门名称(内连接)
select ename,deptname from emp,dept where emp.deptno=dept.deptno;
-- 同
select ename,deptname from emp inner join dept on emp.deptno=dept.deptno;
/* 外连接有分为左连接和右连接，具体定义如下。
• 左连接：包含所有的左边表中的记录甚至是右边表中没有和它匹配的记录
• 右连接：包含所有的右边表中的记录甚至是左边表中没有和它匹配的记录
*/

-- 子查询
/* 某些情况下，当我们查询的时候，需要的条件是另外一个 select 语句的结果，这个时候，就
要用到子查询。用于子查询的关键字主要包括 in、not in、=、!=、exists、not exists 等。
*/
-- 从 emp 表中查询出所有部门在 dept 表中的所有记录：
 select * from emp where deptno in(select deptno from dept);
-- 某些情况下，子查询可以转化为表连接
 select emp.* from emp ,dept where emp.deptno=dept.deptno;

-- 记录联合
SELECT * FROM t1
UNION|UNION ALL
SELECT * FROM t2
……
UNION|UNION ALL
SELECT * FROM tn;
/* UNION 和 UNION ALL 的主要区别是 UNION ALL 是把结果集直接合并在一起，而 UNION 是将
UNION ALL 后的结果进行一次 DISTINCT，去除重复记录后的结果。
*/
```

## DCL

```sql
-- 创建一个数据库用户 z1，具有对 sakila 数据库中所有表的 SELECT/INSERT 权限
grant select,insert on sakila.* to 'z1'@'localhost' identified by '123';
-- 回收插入权限
revoke insert on sakila.* from 'z1'@'localhost';
```


## 帮助的使用
?和help一样:
显示所有可供查询的的分类:
? contents
help contents

http://dev.mysql.com/downloads/是 MySQL 的官方网站，可以下载到各个版本的 MySQL 以及
相关客户端开发工具等。
http://dev.mysql.com/doc/ᨀ供了目前最权威的 MySQL 数据库及工具的在线手册。
http://bugs.mysql.com/这里可以查看到 MySQL 已经发布的 bug 列表，或者向 MySQL ᨀ交 bug
报告。
http://www.mysql.com/news-and-events/newsletter/通常会发布各种关于MySQL的最新消息。

## 小结
本章简单地介绍了 SQL 语句的基本分类 DML/DDL/DCL，并对每一种分类下的常用 SQL 的用
法进行了举例说明。MySQL 在标准 SQL 的基础上进行了很多扩展，本章对常用的一些语法
做了简单介绍，更详细的说明，读者可以参考 MySQL 的帮助或者官方文档。在本章的最后，
还介绍了用户应如何使用 MySQL 中的帮助文档，以便快速查找各种语法定义。

