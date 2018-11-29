---
title: 'linux常用指令(一):链接指令'
categories:
  - linux
tags:
  - linux
  - linux操作系统
  - linux常用指令
date: 2018-11-29 16:59:17
---
# linux常用指令(一):链接指令

## 1. 文件链接的定义

### 1.1 什么是文件链接,要从文件储存说起。

在linux系统中,文件储存在硬盘上，硬盘的最小存储单位叫做"扇区"（Sector）。每个扇区储存512字节（相当于0.5KB）。

操作系统读取硬盘的时候，不会一个个扇区地读取，这样效率太低，而是一次性连续读取多个扇区，即一次性读取一个"块"（block）。这种由多个扇区组成的"块"，是文件存取的最小单位。"块"的大小，最常见的是4KB，即连续八个 sector组成一个 block。

文件数据都储存在"块"中，那么很显然，我们还必须找到一个地方储存文件的元信息，比如文件的创建者、文件的创建日期、文件的大小等等。这种储存文件元信息的区域就叫做inode，中文译名为"索引节点"。

**每一个文件一定有一个inode，但一个inode可以对应多个文件(这就是硬链接)。**

由于每个文件都必须有一个inode，因此有可能发生inode已经用光，但是硬盘还未存满的情况。这时，就无法在硬盘上创建新文件。

每个inode都有一个号码，操作系统用inode号码来识别不同的文件。

这里值得重复一遍，Unix/Linux系统内部不使用文件名，而使用inode号码来识别文件。对于系统来说，文件名只是inode号码便于识别的别称或者绰号。

表面上，用户通过文件名，打开文件。实际上，系统内部这个过程分成三步：首先，系统找到这个文件名对应的inode号码；其次，通过inode号码，获取inode信息；最后，根据inode信息，找到文件数据所在的block，读出数据。

`查看文件的inode号码`
```sh
ls -i filename
```

`查看文件的inode信息`
```sh
stat filename
```

## 2. 链接指令描述

 ln: link的缩写

>`命令所在路径`：/bin/link

>`执行权限`：所有用户

>`功能描述`：生成链接文件

>`语法`： ln -s 【源文件】【目标文件】
　　　　　  -s 　　 创建软链接
　　　　　不加 -s   创建硬链接

### 2.1 硬链接

上面说过,多个文件名指向同一个inode号码的情况称为硬链接(hard link)

也就是说,可以用不同的文件名访问同样的内容(内容实际上在存储上只有一份),对其中一份文件的修改会影响其他所有文件

`创建硬链接`
```sh
ln originalFileName targetFileName
```

e.g.

创建文件 /etc/issue 的硬链接 /tmp/issue.hard：
```sh
ln  /etc/issue /tmp/issue.hard
```

创建硬链接后,inode节点信息中链接数加1,当inode节点的链接数为0,系统会回收inode,释放对应的block区块

### 2.2 软链接

文件A的内容是文件B的路径,这种情况较软链接(soft link)

文件A是依存文件B的,删除文件B,打开A会`no such file or directoroy`

`创建软链接`
```sh
ln -s originalFileName targetFileName
```

e.g.

创建文件 /etc/issue 的软链接 /tmp/issue.soft：
```sh
ln -s /etc/issue /tmp/issue.soft
```


**软链接的硬链接的区别:**

1.`软链接`是文件A指向文件B

2. `硬链接`是文件A和文件B都指向同一个inode

3. 软链接 前面是 l 开头的（link）,而硬链接是 - 开头，表示文件

4. 软链接类似与 windows 的快捷方式，有一个明显的箭头指向，而指向的是源文件

5. 通过 ls -i 操作，来查看 文件的 i 节点。发现硬链接和源文件的 i 节点是相同的，而软链接与源文件的 i 节点是不同的

6. 不允许将硬链接指向目录；不允许跨分区创建硬链接

以上参考: [https://www.cnblogs.com/ysocean/p/7712425.html](https://www.cnblogs.com/ysocean/p/7712425.html)

[https://www.cnblogs.com/ysocean/p/7712417.html](https://www.cnblogs.com/ysocean/p/7712417.html)

[https://blog.csdn.net/cc_946079647/article/details/19088205](https://blog.csdn.net/cc_946079647/article/details/19088205) 其中有关于目录文件的解释
