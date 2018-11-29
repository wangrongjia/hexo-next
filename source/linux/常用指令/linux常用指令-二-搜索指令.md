---
title: 'linux常用指令(二):搜索指令'
categories:
  - linux
tags:
  - linux
  - linux操作系统
  - linux常用指令
date: 2018-11-29 16:59:25
---
# linux常用指令(二):搜索指令
## 文件或目录搜索命令(find)

### 1. 命令简介

>命令所在路径：/bin/find

>执行权限：所有用户

>语法：find【搜索范围】【匹配条件】

搜索范围就是路径,匹配条件有很多种

### 2. 匹配条件的几种用法

#### 2.1 根据 文件或目录名称 搜索

find 【搜索目录】【-name或者-iname】【搜索字符】：-name和-iname的区别一个区分大小写，一个不区分大小写,搜索字符支持通配符(*:0个或多个字符,?:一个字符)

>find /etc -name init   (精准搜索，名字必须为 init 才能搜索的到)

>find /etc -name *init  (模糊搜索，以 init 结尾的文件或目录名) 

>find /etc -name init??? (模糊搜索，？ 表示单个字符，即搜索到 init___)

#### 2.2 根据 文件大小 搜索

比如：在根目录下查找大于 100M 的文件

find / -size +204800

这里 +n 表示大于，-n 表示小于，n 表示等于

1 数据块 == 512 字节 ==0.5KB，也就是1KB等于2数据块

100MB == 102400KB==204800数据块

#### 2.3 根据所有者或用户组

`在home目录下查询所属组为 root 的文件`
```sh
find /home -group root
```
`在home目录下查询所有者为 root 的文件`
```sh
find /home -user root
```

## 命令搜索

### whereis
 
>命令名称：whereis

>命令所在路径：/usr/bin/whereis

>执行权限：所有用户

>功能描述：搜索命令所在的目录及帮助文档路径

>语法：whereis【命令】

`查询 ls 命令所在目录以及帮助文档路径`
```sh
whereis ls
```

### which

搜索命令所在的目录及别名信息

## 文件内搜索

>命令名称：grep

>命令所在路径：/bin/grep

>执行权限：所有用户

>功能描述：在文件中搜寻字符串匹配的行并输出

>语法：grep -iv 【指定字符串】【文件】
　　　　　　　　-i 不区分大小写
　　　　　　　　-v 排除指定字符串

`查找 /root/install.log 文件中包含 mysql 字符串的行，并输出`
```sh
grep mysql /root/install.log
```

参考:[https://www.cnblogs.com/ysocean/p/7712417.html](https://www.cnblogs.com/ysocean/p/7712417.html)