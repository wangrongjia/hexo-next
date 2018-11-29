---
title: '深入浅出MySQL阅读笔记二:数据类型'
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
  - MySQL数据类型
date: 2018-11-29 15:41:37
---
# 深入浅出MySQL阅读笔记二:数据类型
## 1. 数值类型
MySQL 支持所有标准 SQL 中的数值类型，其中包括严格数值类型（INTEGER、SMALLINT、
DECIMAL 和 NUMERIC），以及近似数值数据类型（FLOAT、REAL 和 DOUBLE PRECISION），并
在此基础上做了扩展。扩展后增加了 TINYINT、MEDIUMINT 和 BIGINT 这 3 种长度不同的整
型，并增加了 BIT 类型，用来存放位数据。表 3-1 中列出了 MySQL 5.0 中支持的所有数值类
型，其中 INT 是 INTEGER 的同名词，DEC 是 DECIMAL 的同名词。

整数类型 | 字节 | 最小值 | 最大值
--|--|--|--
TINYINT | 1 | 有符号-128,无符号0 |有符号127,无符号255
SMALLINT | 2 | 有符号-32768,无符号0 | 有符号32767,无符号65535
MEDIUMINT | 3 | 有符号-8388608,无符号0 | 有符号8388607,无符号1677215
INT、INTEGER | 4 | 有符号-2147483648,无符号0 | 有符号2147483647,无符号4294967295
BIGINT | 8 | 有符号-9223372036854775808,无符号0 | 有符号9223372036854775807,无符号18446744073709551615

浮点数类型 | 字节 | 最小值 | 最大值
--|--|--|--
FLOAT | 4 | ±1.175494351E-38 | ±3.402823466E+38
DOUBLE | 8 | ±2.2250738585072014E-308 | ±1.7976931348623157E+308

定点数类型 | 字节 | 描述
--|--|--
DEC(M,D),DECIMAL(M,D) | M+2 | 最大取值范围与 DOUBLE 相同,给定 DECIMAL 的有效取值范围由 M 和 D决定

位类型 | 字节 | 最小值 | 最大值
--|--|--|--
BIT(M) | 1～8 | BIT(1) | BIT(64)

### 1.1 整数
对于整型数据，MySQL 还支持在类型名称后面的小括号内指定显示宽度，例如 int(5)表
示当数值宽度小于 5 位的时候在数字前面填满宽度，如果不显示指定宽度则默认为 int(11)。
一般配合 zerofill 使用，顾名思义，zerofill 就是用“0”填充的意思，也就是在数字位数不够
的空间用字符“0”填满,也就是左边补0

```sql
-- 建立测试表
create table t1 (
id1 int zerofill,
id2 int(5) zerofill);
-- 插入数据
insert into t1 values(1,1),(111111,111111);
```
`结果`:
```
mysql> select * from t1;
+------------+--------+
| id1        | id2    |
+------------+--------+
| 0000000001 |  00001 |
| 0000111111 | 111111 |
+------------+--------+
```
`结论`:
- int 默认int(11)
- 插入大于宽度限制的值,不会截取;int(5)可以插入111111
- 另外,指定为zerofill,则 MySQL 自动为该列添加 `UNSIGNED` 属性

另外，整数类型还有一个属性：AUTO_INCREMENT。在需要产生唯一标识符或顺序值时，
可利用此属性，这个属性只用于整数类型。AUTO_INCREMENT 值一般从 1 开始，每行增加 1。

一个表中最多只能有一个AUTO_INCREMENT列。

对于任何想要使用AUTO_INCREMENT 的
列，应该定义为 NOT NULL，并定义为 PRIMARY KEY 或定义为 UNIQUE 键
### 1.2 小数

对于小数的表示，MySQL 分为两种方式：`浮点数`和`定点数`。浮点数包括 float（单精度）
和 double（双精度），而定点数则只有 decimal 一种表示。定点数在 MySQL 内部以字符串形
式存放，比浮点数更精确，适合用来表示货币等精度高的数据。

浮点数和定点数都可以用类型名称后加“(M,D)”的方式来进行表示,(整数位+小数位位数,小数位位数)
整数位是M-D,小数位是D

MySQL 保存值时进行四舍五入，因此如果在 float(7,4)列内插入 999.00009，近似结果是 999.0001

值得注意的是，浮点数后面跟“(M,D)”的用法是非标准用法，如果要用于数据库的迁移，则最好不要这么使用。

float 和 double 在不指定精度时，默认会按照实际的精度（由实际的硬件和操作系统决定）
来显示，而 decimal 在不指定精度时，**默认(10,0)**。

**浮点数的保存,小数位基本上一定损失精度,除非是0.5,0.25,0.5+0.25这种,所以如果涉及计算,尽量避免使用浮点数,使用decimal**
e.g.
```sql
-- 建表
drop table if exists t1;
create table t1(
id float(8,5)
);
-- 插入数据
insert into t1 values(999.00999);
```
`结果`:
```
mysql> select * from t2;
+------------+
| id         |
+------------+
| 999.010010 |
+------------+
1 row in set (0.00 sec)
```
看起来很奇怪的...

### 1.3 位类型(bit)

```sql
-- 建表
drop table if exists t2;
create table t2(
id bit(4)
);
-- 插入数据
insert into t2 values(2);
```
`结果`:
```
mysql> select * from t2;
+------+
| id   |
+------+
|     |
+------+
1 row in set (0.00 sec)
```
不会显示数值,需要用bin(),或者hex()[十六进制]取出
```
mysql> select bin(id),hex(id) from t2;
+---------+---------+
| bin(id) | hex(id) |
+---------+---------+
| 10      | 2       |
+---------+---------+
1 row in set (0.00 sec)
```

## 2. 日起时间类型
MySQL 5.0 中所支持的日期和时间类型

日期和时间类型 | 字节 | 最小值 | 最大值
--|--|--|--
DATE | 4 | 1000-01-01 | 9999-12-31
DATETIME | 8 | 1000-01-01 00:00:00 | 9999-12-31 23:59:59
TIMESTAMP | 4 | 19700101080001 | 2038 年的某个时刻
TIME | 3 | -838:59:59 | 838:59:59
YEAR | 1 | 1901 |2155

如果要用来表示年月日时分秒，通常用 DATETIME 表示。

如果需要**经常插入或者更新日期**为当前系统时间，则通常使用 TIMESTAMP 来表示。
TIMESTAMP 值返回后显示为“YYYY-MM-DD HH:MM:SS”格式的字符串，显示宽度固定
为 **19 个字符**。如果想要获得数字值，应在 TIMESTAMP 列添加+0。

每种日期时间类型都有一个有效值范围，如果超出这个范围，在
默认的 SQLMode 下，系统会进行错误提示，并将以零值来进行存储。

数据类型 | 零值表示
--|--
DATETIME | 0000-00-00 00:00:00
DATE | 0000-00-00
TIMESTAMP | 00000000000000
TIME | 00:00:00
YEAR | 0000

MySQL会给表中的第一个TIMESTAMP字段设置默认值为系统日期，如果有第二个TIMESTAMP类型，则默认值设置为0值 
```sql
-- 建表
drop table if exists t2;
create table t2(
ts1 timestamp,
ts2 timestamp
);
--- 实际的sql语句
CREATE TABLE `t2` (
	`ts1` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	`ts2` TIMESTAMP NOT NULL DEFAULT '0000-00-00 00:00:00'
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;
```
可见,timestamp类型的字段在记录修改的时候也会默认的更新为当前系统时间

TIMESTAMP还有一个重要特点，就是**和时区相关。当插入日期时，会先转换为本地时区
后存放；而从数据库里面取出时，也同样需要将日期转换为本地时区后显示。

```sql
-- 查看当前时区
 show variables like 'time_zone';
```
`结果`:
```
mysql>  show variables like 'time_zone';
+---------------+--------+
| Variable_name | Value  |
+---------------+--------+
| time_zone     | SYSTEM |
+---------------+--------+
1 row in set (0.00 sec)
```
默认系统时区,中国东八时区(+8:00)
```sql
-- 建表
drop table if exists t2;
CREATE TABLE `t2` (
 `id1` timestamp NOT NULL default CURRENT_TIMESTAMP,
 `id2` datetime default NULL
);
-- 插入数据
insert into t2 values(now(),now());
-- 修改时区
 set time_zone='+9:00';
```
`结果`:
```
mysql> select * from t2;
+---------------------+---------------------+
| id1                 | id2                 |
+---------------------+---------------------+
| 2018-11-07 20:05:02 | 2018-11-07 19:05:02 |
+---------------------+---------------------+
```
注意:set time_zone = '+9:00' 这种设置时区的方式只对当前session有效

从上面例子可以看出，TIMESTAMP和DATETIME的表示方法非常类似，区别主要有以下
几点。
-  TIMESTAMP支持的时间范围较小，其取值范围从19700101080001到2038年的某个
时间，而DATETIME是从1000-01-01 00:00:00到9999-12-31 23:59:59，范围更大。
- 表中的第一个TIMESTAMP列自动设置为系统时间。如果在一个TIMESTAMP列中插入
NULL，则该列值将自动设置为当前的日期和时间。在插入或更新一行但不明确给
TIMESTAMP列赋值时也会自动设置该列的值为当前的日期和时间，当插入的值超出
取值范围时，MySQL认为该值溢出，使用“0000-00-00 00:00:00”进行填补。
- TIMESTAMP的插入和查询都受当地时区的影响，更能反应出实际的日期。而
DATETIME则只能反应出插入时当地的时区，其他时区的人查看数据必然会有误差
的。
- TIMESTAMP的属性受MySQL版本和服务器SQLMode的影响很大，本章都是以MySQL
5.0为例进行介绍，在不同的版本下可以参考相应的MySQL帮助文档

## 3. 字符串类型
5.0版本

字符串类型 | 字节 | 描述及存储需求
--|--|--
CHAR（M） | M | M 为 0～255 之间的整数
VARCHAR（M） | M | 为 0～65535 之间的整数，值的长度+1 个字节
TINYBLOB |  |允许长度 0～255 字节，值的长度+1 个字节
BLOB |  |允许长度 0～65535 字节，值的长度+2 个字节
MEDIUMBLOB |  | 允许长度 0～167772150 字节，值的长度+3 个字节
LONGBLOB |  | 允许长度 0～4294967295 字节，值的长度+4 个字节
TINYTEXT |  | 允许长度 0～255 字节，值的长度+2 个字节
TEXT |  | 允许长度 0～65535 字节，值的长度+2 个字节
MEDIUMTEXT |  | 允许长度 0～167772150 字节，值的长度+3 个字节
LONGTEXT |  | 允许长度 0～4294967295 字节，值的长度+4 个字节
VARBINARY（M） |  | 允许长度 0～M 个字节的变长字节字符串，值的长度+1 个字节
BINARY（M） | M | 允许长度 0～M 个字节的定长字节字符串

### 3.1 char和varchar
- 存储方式上:char定长,varchar可变长
- char会删除字符串尾部的空格,varchar不会

### 3.2 enum类型
```sql
 create table t (gender enum('M','F'));
```
插入的时候,插入其他值,默认插入枚举项的第一个

### 3.3 set类型
```sql
Create table t (col set （'a','b','c','d'）;
-- set和enum区别是set可以插入多个值
insert into t values('a,b'),('a,d,a'),('a,b'),('a,c'),('a');
```
不在范围内的值,不允许插入

去重,('a','b','a')-->('a','b')

# 小结

本章主要介绍了 MySQL 支持的各种数据类型，并通过多个实例对它们的使用方法做了详细
的说明。学完本章后，读者可以对每种数据类型的用途、物理存储、表示范围等有一个概要
的了解。这样在面对具体应用时，就可以根据相应的特点来选择合适的数据类型，使得我们
能够争取在满足应用的基础上，用较小的存储代价换来较高的数据库性能。
