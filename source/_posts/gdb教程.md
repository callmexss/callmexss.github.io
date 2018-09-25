---
title: gdb教程「翻译」
date: 2018-09-22 10:09:15
categories: 翻译
tags:
  - gdb
  - debug

---

> 原文[Debugging Under Unix: gdb Tutorial](https://www.cs.cmu.edu/~gilpin/tutorial/)

## 在Unix 下调试： gdb 教程

因为最近做了一个项目需要在linux环境下编译c++程序，于是把源码迁移到了Ubuntu，然后发现在修改了代码后程序报了Segment Fault的错误。这不通过调试只是静态分析怎么可能找到，于是被逼无奈又只能谷歌大法搜呗...于是就搜到了这篇04年的教程。gdb我之前好奇用了一次，但是完全不知所以，这次跟着这篇教程却算是顺利的入了门。虽然最终bug不是通过gdb找到的，不过为了纪念，还是准备把这篇我认为还不错的教程翻译下来留作纪念。

### 介绍
本教程最初是为华盛顿大学 CS 342 而撰写。它现在仍旧由 Andrew Gilpin 进行维护。
#### 写给谁看？
本教程旨在帮助新接触UNIX环境的程序员学习使用gdb调试器。本教程假设你已经能够使用C++编写程序并知道如何编译运行。它还假设你了解关于调试的基础知识，并在其它操作系统中使用过调试器。
#### 源码
为了阐述调试的原理，我将使用一个有bug的程序作为例子。在你读完本教程后，你将能够使用调试器定位和修复代码中的错误。代码在[这里](https://www.cs.cmu.edu/~gilpin/tutorial/main.cc)下载，以及一个简单的[Makefile](https://www.cs.cmu.edu/~gilpin/tutorial/Makefile)。

代码非常简单，包含一个节点和一个链表的定义。这也是测试列表的一个简单开始。为了能够更容易的阐述调试的过程，所有的代码被放在了一个文件中。

**注意:** 由于代码比较久远，其中有一些需要修改的地方，一份[修改后的代码]()放到github仓库中了。Makefile比较短，贴在这里学习一下：
```sh
CXX = g++ # 指定编译器
FLAGS = -ggdb -Wall # 使用调试模式

main: main.cc # 函数入口点
	${CXX} ${FLAGS} -o main main.cc # 编译命令

clean:
	rm -f main # 删除链接文件  
```
### 准备工作
#### 配置环境
gdb包含在CEC（没查到是什么...）机器的GNU包中。如果你没有加载这个包，在命令行输入`pkgadd gnu`。如果你可以运行g++，那么你就可以运行gdb。（没有gdb环境的使用包管理工具安装一下。）
#### 调试符号
gdb只能作用于由g++产生的调试符号。对于SUN CC的用户，有一个和gdb很相似的替代品叫做dbx调试器。
### 调试
#### 什么时候该使用调试器
调试是编程过程中不可避免的。每一个程序员在他们的编程生涯中都将会有某个时刻需要使用调试器调试一段代码。有许多调试的方法，从在屏幕上打印信息，到使用调试器，或者只是思考程序在干什么，并对问题是什么进行有根据的猜测。修复一个bug前，必须先定位bug的位置。例如，在段错误中，知道在哪一行产生段错误是非常有用的。一旦出问题的代码被找到了，知道那个方法中的数值、谁调用了那个方法、以及为什么这个错误会发生都将是有帮助的。使用调试器会使找到这些信息变得十分简单。

继续阅读本教程并使用make编译以及运行这个程序。本程序将会打印出一些信息，然后会打印它出现了一个段错误，导致程序崩溃。仅根据屏幕上显示的信息，几乎不可能找到程序崩溃的原因，更不用说修复这个程序了。我们现在将开始调试这个程序。
#### 加载程序
现在你有一个了一个可执行文件（本例中是main）然后你想调试它。首先你必须运行调试器，调试器叫做gdb，你必须在命令行中告诉他你想要调试哪一个程序。所以为了调试我们输入 `gdb main` 。这里是运行后可能出现的样子：
```sh
agg1@sukhoi agg1/.www-docs/tutorial> gdb main
GNU gdb 4.18
Copyright 1998 Free Software Foundation, Inc.
GDB is free software, covered by the GNU General Public License, and you are
welcome to change it and/or distribute copies of it under certain conditions.
Type "show copying" to see the conditions.
There is absolutely no warranty for GDB.  Type "show warranty" for details.
This GDB was configured as "sparc-sun-solaris2.7"...
(gdb)

```

一段关于emacs的内容就跳过了。

gdb 现在等着接收用户指令。我们需要运行程序，这样gdb可以帮助我们观察程序崩溃时到底发生了什么。在gdb命令行中输入 `run`。这里是输入 `run` 后发生的事情：

```sh
(gdb) run
Starting program: /home/cec/s/a/agg1/.www-docs/tutorial/main 
Creating Node, 1 are in existence right now
Creating Node, 2 are in existence right now
Creating Node, 3 are in existence right now
Creating Node, 4 are in existence right now
The fully created list is:
4
3
2
1

Now removing elements:
Creating Node, 5 are in existence right now
Destroying Node, 4 are in existence right now
4
3
2
1


Program received signal SIGSEGV, Segmentation fault.
Node<int>::next (this=0x0) at main.cc:28
28	  Node<T>* next () const { return next_; }
(gdb)

```

程序崩溃了，让我们来看看从输入结果中我们能收集到哪些信息。

**注意：** 调试的程序一定是编译好的二进制文件。

#### 检查崩溃

#### 条件断点
#### 单步运行
### 更多信息
### 注意