---
title: '深入浅出MySQL阅读笔记四:常用函数'
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
  - MySQL常用函数
date: 2018-11-29 15:41:47
---
# 深入浅出MySQL阅读笔记四:常用函数
## 1. 字符串函数

函数 | 功能
--|--
CANCAT(S1,S2,…Sn) | 连接 S1,S2,…Sn 为一个字符串
INSERT(str,x,y,instr) | 将字符串 str 从第 x 位置开始，y 个字符长的子串替换为字符串 instr,会覆盖原有的
LOWER(str) | 将字符串 str 中所有字符变为小写
UPPER(str) | 将字符串 str 中所有字符变为大写
LEFT(str ,x) | 返回字符串 str 最左边的 x 个字符
RIGHT(str,x) | 返回字符串 str 最右边的 x 个字符
LPAD(str,n ,pad) | 用字符串 pad 对 str 最左边进行填充，直到长度为 n 个字符长度
RPAD(str,n,pad) | 用字符串 pad 对 str 最右边进行填充，直到长度为 n 个字符长度
LTRIM(str) | 去掉字符串 str 左侧的空格
RTRIM(str) | 去掉字符串 str 行尾的空格
REPEAT(str,x) | 返回 str 重复 x 次的结果
REPLACE(str,a,b) | 用字符串 b 替换字符串 str 中所有出现的字符串 a
STRCMP(s1,s2) | 比较字符串 s1 和 s2 的 ASCII 码值的大小。如果 s1 比 s2 小，那么返回-1;如果 s1 与 s2 相等，那么返回 0；如果 s1 比 s2 大，那么返回 1。
TRIM(str) | 去掉字符串行尾和行头的空格
SUBSTRING(str,x,y) | 返回从字符串 str x 位置起 y 个字符长度的字串

tips:
**mysql索引的起始位置是1,不是0**

## 2. 数值函数

函数 | 功能
--|--
ABS(x) | 返回 x 的绝对值
CEIL(x) | 返回大于 x 的最大整数值
FLOOR(x) | 返回小于 x 的最大整数值
MOD(x，y) | 返回 x/y 的模
RAND() | 返回 0 到 1 内的随机值
ROUND(x,y) | 返回参数 x 的四舍五入的有 y 位小数的值
TRUNCATE(x,y) | 返回数字 x 截断为 y 位小数的结果

```
-- 取0~100内的随机整数
mysql> select round(100*rand()),round(100*rand());
+-------------------+-------------------+
| round(100*rand()) | round(100*rand()) |
+-------------------+-------------------+
|                55 |                43 |
+-------------------+-------------------+
-- round(x,y)和truncate(x,y)
mysql> select round(1.23456,4),truncate(1.23456,4);
+------------------+---------------------+
| round(1.23456,4) | truncate(1.23456,4) |
+------------------+---------------------+
|           1.2346 |              1.2345 |
+------------------+---------------------+
```
## 3. 日期和时间函数

函数 | 功能
--|--
CURDATE() | 返回当前日期
CURTIME() | 返回当前时间
NOW() | 返回当前的日期和时间
UNIX_TIMESTAMP(date) | 返回日期 date 的 UNIX 时间戳
FROM_UNIXTIME(unix_timestamp,format) | 返回 UNIX 时间戳的日期值
WEEK(date) | 返回日期 date 为一年中的第几周
YEAR(date) | 返回日期 date 的年份
HOUR(time) | 返回 time 的小时值
MINUTE(time) | 返回 time 的分钟值
MONTHNAME(date) | 返回 date 的月份名
DATE_FORMAT(date,fmt) | 返回按字符串 fmt 格式化日期 date 值
DATE_ADD(date,INTERVAL expr type) | 返回一个日期或时间值加上一个时间间隔的时间值
DATEDIFF(expr,expr2) | 返回起始时间 expr 和结束时间 expr2 之间的天数

- UNIX_TIMESTAMP():没有参数,返回当前时间戳
- DATE_FORMAT(date,fmt):按字符串 fmt 格式化日期 date 值，此函数能够按指定的
格式显示日期，可以用到的格式符如表所示

格式符 | 格式说明
--|--
%S,%s | 两位数字形式的秒（00,01,...,59）
%i | 两位数字形式的分（00,01,...,59）
%H | 两位数字形式的小时，24 小时（00,01,...,23）
%h,%I | 两位数字形式的小时，12 小时（01,02,...,12）
%k | 数字形式的小时，24 小时（0,1,...,23）
%l | 数字形式的小时，12 小时（1,2,...,12）
%T | 24 小时的时间形式（hh:mm:ss）
%r | 12 小时的时间形式（hh:mm:ssAM 或 hh:mm:ssPM）
%p | AM 或 PM
%W | 一周中每一天的名称（Sunday,Monday,...,Saturday）
%a | 一周中每一天名称的缩写（Sun,Mon,...,Sat）
%d | 两位数字表示月中的天数（00,01,...,31）
%e | 数字形式表示月中的天数（1,2，...,31）
%D | 英文后缀表示月中的天数（1st,2nd,3rd,...）
%w | 以数字形式表示周中的天数（0=Sunday,1=Monday,...,6=Saturday）
%j | 以 3 位数字表示年中的天数（001,002,...,366）
%U | 周（0,1,52），其中 Sunday 为周中的第一天
%u | 周（0,1,52），其中 Monday 为周中的第一天
%M | 月名（January,February,...,December）
%b | 缩写的月名（January,February,...,December）
%m | 两位数字表示的月份（01,02,...,12）
%c | 数字表示的月份（1,2,...,12）
%Y | 4 位数字表示的年份
%y | 两位数字表示的年份
%% | 直接值“%”

`一般格式`:
```
mysql> select date_format(now(),'%Y-%m-%d %H:%i:%s');
+----------------------------------------+
| date_format(now(),'%Y-%m-%d %H:%i:%s') |
+----------------------------------------+
| 2018-11-08 11:06:17                    |
+----------------------------------------+
```

- DATE_ADD(date,INTERVAL expr type):其中 INTERVAL 是间隔类型关键字，expr 是一个表达式，这个表达式对应后面的类型，type是间隔类型，MySQL提供了 13 种间隔类型，如表所示

表达式类型 | 描述 | 格式
--|--|--
HOUR | 小时 | hh
MINUTE | 分 | mm
SECOND | 秒 | ss
YEAR | 年 | YY
MONTH | 月 | MM
DAY | 日 | DD
YEAR_MONTH | 年和月 | YY-MM
DAY_HOUR | 日和小时 | DD hh
DAY_MINUTE | 日和分钟 | DD hh:mm
DAY_ SECOND | 日和秒 | DD hh:mm:ss
HOUR_MINUTE | 小时和分 | hh:mm
HOUR_SECOND | 小时和秒 | hh:ss
MINUTE_SECOND | 分钟和秒 | mm:ss

```
mysql> select now() current, date_add(now(),interval '10-6' year_month) after10y
ear6month,date_add(now(),interval '01 01:01:01' day_second) after1day1h1m1s;
+---------------------+---------------------+---------------------+
| current             | after10year6month   | after1day1h1m1s     |
+---------------------+---------------------+---------------------+
| 2018-11-08 11:21:43 | 2029-05-08 11:21:43 | 2018-11-09 12:22:44 |
+---------------------+---------------------+---------------------+
```

## 4. 流程函数

函数 | 功能
--|--
IF(value,t f) | 如果 value 是真，返回 t；否则返回 f
IFNULL(value1,value2) | 如果 value1 不为空返回 value1，否则返回 value2
CASE WHEN [value1] THEN[result1]…ELSE[default]END | 如果 value1 是真，返回 result1，否则返回 default
CASE [expr] WHEN [value1] THEN[result1]…ELSE[default]END | 如果 expr 等于 value1，返回 result1，否则返回 default

```sql
-- 建表
drop table if exists salary;
create table salary (
userid int,
salary decimal(9,2)
);
-- 插入数据
insert into salary values(1,1000),(2,2000), (3,3000),(4,4000),(5,5000), (1,null);
-- if()
select if(salary>2000,'high','low') from salary;
-- ifnull()
 select ifnull(salary,0) from salary;
-- CASE WHEN
select case when salary<=2000 then 'low' else 'high' end from salary;
-- CASE [expr] WHEN
select case salary when 1000 then 'low' when 2000 then 'mid' else 'high' end from salary;
```

## 5. 其他常用函数

函数 | 功能
--|--
DATABASE() | 返回当前数据库名
VERSION() | 返回当前数据库版本
USER() | 返回当前登录用户名
INET_ATON(IP) | 返回 IP 地址的数字表示
INET_NTOA(num) | 返回数字代表的 IP 地址
PASSWORD(str) | 返回字符串 str 的加密版本
MD5() | 返回字符串 str 的 MD5 值

## 6. 小结

本章主要对 MySQL 常用的各类常用函数通过实例做了介绍。MySQL 有很多内建函数，这些
内建函数实现了很多应用需要的功能并且拥有很好的性能，如果用户在工作中需要实现某种
功能，最好先查一下 MySQL 官方文档或者帮助，看是否已经有相应的函数实现了我们需要
的功能，可以大大ᨀ高工作效率。由于篇幅所限，本章并没有介绍所有的函数，读者可以去
进一步查询相关文档