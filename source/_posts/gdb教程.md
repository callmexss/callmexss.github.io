---
title: gdb教程「翻译」
date: 2018-09-22 10:09:15
categories: 翻译
tags:
  - gdb
  - debug

---

一篇经典入门教程的翻译。

> 原文[Debugging Under Unix: gdb Tutorial](https://www.cs.cmu.edu/~gilpin/tutorial/)

<!-- more -->

**在Unix 下调试： gdb 教程**

因为最近做了一个项目需要在linux环境下编译c++程序，于是把源码迁移到了Ubuntu，然后发现在修改了代码后程序报了Segment Fault的错误。这不通过调试只是静态分析怎么可能找到，于是被逼无奈又只能谷歌大法搜呗...于是就搜到了这篇04年的教程。gdb我之前好奇用了一次，但是完全不知所以，这次跟着这篇教程却算是顺利的入了门。虽然最终bug不是通过gdb找到的，不过为了纪念，还是准备把这篇我认为还不错的教程翻译下来留作纪念。

## 介绍

本教程最初是为华盛顿大学 CS 342 而撰写。它现在仍旧由 Andrew Gilpin 进行维护。

### 写给谁看？

本教程旨在帮助新接触UNIX环境的程序员学习使用gdb调试器。本教程假设你已经能够使用C++编写程序并知道如何编译运行。它还假设你了解关于调试的基础知识，并在其它操作系统中使用过调试器。

### 源码

为了阐述调试的原理，我将使用一个有bug的程序作为例子。在你读完本教程后，你将能够使用调试器定位和修复代码中的错误。代码在[这里](https://www.cs.cmu.edu/~gilpin/tutorial/main.cc)下载，以及一个简单的[Makefile](https://www.cs.cmu.edu/~gilpin/tutorial/Makefile)。

代码非常简单，包含一个节点和一个链表的定义。这也是测试列表的一个简单开始。为了能够更容易的阐述调试的过程，所有的代码被放在了一个文件中。

**注意:** 由于代码比较久远，其中有一些需要修改的地方，一份[修改后的代码](https://raw.githubusercontent.com/callmexss/callmexss.github.io/blog/source/_posts/gdb%E6%95%99%E7%A8%8B/main.cc)放到github仓库中了。Makefile比较短，贴在这里学习一下：

```sh
CXX = g++ # 指定编译器
FLAGS = -ggdb -Wall # 使用调试模式

main: main.cc # 函数入口点
  ${CXX} ${FLAGS} -o main main.cc # 编译命令

clean:
  rm -f main # 删除链接文件  
```

## 准备工作

### 配置环境

gdb包含在CEC（没查到是什么...）机器的GNU包中。如果你没有加载这个包，在命令行输入`pkgadd gnu`。如果你可以运行g++，那么你就可以运行gdb。（没有gdb环境的使用包管理工具安装一下。）

### 调试符号

gdb只能作用于由g++产生的调试符号。对于SUN CC的用户，有一个和gdb很相似的替代品叫做dbx调试器。

## 调试

### 什么时候该使用调试器

调试是编程过程中不可避免的。每一个程序员在他们的编程生涯中都将会有某个时刻需要使用调试器调试一段代码。有许多调试的方法，从在屏幕上打印信息，到使用调试器，或者只是思考程序在干什么，并对问题是什么进行有根据的猜测。修复一个bug前，必须先定位bug的位置。例如，在段错误中，知道在哪一行产生段错误是非常有用的。一旦出问题的代码被找到了，知道那个方法中的数值、谁调用了那个方法、以及为什么这个错误会发生都将是有帮助的。使用调试器会使找到这些信息变得十分简单。

继续阅读本教程并使用make编译以及运行这个程序。本程序将会打印出一些信息，然后会打印它出现了一个段错误，导致程序崩溃。仅根据屏幕上显示的信息，几乎不可能找到程序崩溃的原因，更不用说修复这个程序了。我们现在将开始调试这个程序。

### 加载程序

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
28  Node<T>* next () const { return next_; }
(gdb)

```

程序崩溃了，让我们来看看从输入结果中我们能收集到哪些信息。

**注意：** 调试的程序一定是编译好的二进制文件。

### 检查崩溃

我们现在可以看到程序在main.cc的第28行，`this`指针指向0，这行代码正在被执行。但我们还想知道是谁调用了这个方法，以及调用方法中的值是什么,所以在gdb命令行中输入`backtrace`,得到如下输出：

```sh
(gdb) backtrace
#0  Node<int>::next (this=0x0) at main.cc:28
#1  0x2a16c in LinkedList<int>::remove (this=0x40160,
    item_to_remove=@0xffbef014) at main.cc:77
#2  0x1ad10 in main (argc=1, argv=0xffbef0a4) at main.cc:111
(gdb)
```

所以除了当前方法和局部变量外，我们还可以知道哪些方法调用了我们以及它们的参数是什么。例如，我们可以看出当前方法是由`LinkedList<int>::remove()`调用，参数`item_to_remove`在内存中的地址为`0xffbef014`。知道`item_to_remove`的值或许有助于我们理解bug,所以我们想要看到`item_to_remove`的地址中的值。可以通过使用`x + address`输入到命令行中查看该变量的值(x可以被理解为examine的简写)。这是输入后的效果：

```sh
(gdb) x 0xffbef014
0xffbef014: 0x00000001
(gdb)
```

所以程序实在调用`LinkedList<int>::remove`方法以及参数为1时崩溃的。我们现在把问题缩小到了一个特定的方法和一个特定的参数。

### 条件断点

既然我们已经知道了段错误何时在何处发生，我们想要知道程序在崩溃以前发生了什么。一种方法是使用逐步运行，一次一步,执行程序的全部语句直到执行到我们想要看到的地方。这是有效的，但有时你希望它恰好运行到特定的位置并在那里停止，这样你就可以检测此时的数据。

如果你曾经使用过调试器，那么你或许对断点的概念很熟悉。简单来说，断点就是调试器在源码中应该停止执行的那一行。我们的例子中，我们想要查看`LinkedList<int>::remove ()`中的代码，所以我们想要在main.cc的第52行中设置断点。因为你可能不知道具体的行号，你也可以告诉调试器你想要设置断点的方法。这是在本例中我们的输入：

```sh
(gdb) break LinkedList<int>::remove
Breakpoint 1 at 0x29fa0: file main.cc, line 52.
(gdb)
```

现在断点就如我们所期望的被设置在了main.cc的52行。(断点绑定了一个编号是为了我们之后可以引用它，例如删除这个断点。)每次程序运行时，它会在每次运行到第52行的时候将控制权交还给调试器。当方法会被调用多次但是只会在某个特定的值触发错误时，这样就不优雅了。在本例中，我们知道当参数为1时调用`LinkedList<int>::remove()`方法会导致程序崩溃。所以我们或许想要告诉调试器在`item_to_remove`值为1时中断。这可以通过如下命令实现：

```sh
(gdb) condition 1 item_to_remove==1
(gdb)
```

这等同于告诉调试器“只有在`item_to_remove`值为1时中断。”现在我们可以运行程序，然后看到调试器只有会在某个特定条件为真时进入断点。

### 单步运行

继续上面的例子，我们现在已经设置了一个条件断点，然后让程序每次运行一步，看是否能够定位错误的源头。可以通过`step`命令完成这个过程。gdb有一个很棒的特性，不输入命令直接按下回车时，会自动执行上一条命令。也就是说，在第一次输入`step`后我们就可以通过敲击回车让调试器继续执行`step`命令了。这看起来就像：

```sh
Breakpoint 1, LinkedList<int>::remove (this=0x40160,
    item_to_remove=@0xffbef014) at main.cc:52
52     Node<T> *marker = head_;
(gdb) step
53      Node<T> *temp = 0;  // temp points to one behind as we iterate
(gdb)
55      while (marker != 0) {
(gdb)
56        if (marker->value() == item_to_remove) {
(gdb)
Node<int>::value (this=0x401b0) at main.cc:30
30    const T& value () const { return value_; }
(gdb)
LinkedList<int>::remove (this=0x40160, item_to_remove=@0xffbef014)
    at main.cc:75
75        marker = 0;  // reset the marker
(gdb)
76        temp = marker;
(gdb)
77        marker = marker->next();
(gdb)
Node<int>::next (this=0x0) at main.cc:28
28    Node<T>* next () const { return next_; }
(gdb)

Program received signal SIGSEGV, Segmentation fault.
Node<int>::next (this=0x0) at main.cc:28
28    Node<T>* next () const { return next_; }
(gdb)

```

在键入`run`后，gdb问我们是否要重新运行程序，我们选择是。然后程序会运行并在我们期望的位置中断。我们输入`step`和敲击回车使程序单步运行。注意程序会进入到调用的方法中。如果你不希望这样，使用`next`方法，它会将函数的调用视为一步执行。

程序的错误很明显了。在第75行`marker`被置为0,但是在第77行`marker`的一个成员被访问，**访问没有初始化的对象是危险的**。因为程序不能方位地址为0的内存，所以段错误发生了。在这个例子中，在本例中，`marker`并不用做什么，可以简单的通过移除main.cc的第75行来避免这个错误。

如果你观察程序的输入，你会发现程序会先正常运行一段时间，但是程序中某处会产生内存泄漏。（提示：它就在`LinkedList<T>::remove()`方法里，某次remove没有正常工作时产生）。这将给读者留作使用调试器定位和修复bug的练习。（我总是喜欢这么说.;）

gdb通过输入`quit`退出。

## 更多信息

本文档只涉及使用gdb的最小知识。可以通过gdb的manpage或者看[这份非常长的关于gdb的介绍](http://sources.redhat.com/gdb/current/onlinedocs/gdb_toc.html)可以通过在运行gdb时输入`help`获取在线命令。此外，一如往常，欢迎在新闻组里提问或者在工作时间来问我。

## 注意

- 上面链表的代码中还有一个未被提到的错误。这个错误在按照原始代码中的顺序添加和移除元素时不会发生，但是在其它顺序下会产生。例如，插入1，2，3，和 4，然后尝试删除2时会产生。非常感谢Linda Gu 和 Xiaofeng Chen发现这个错误。这个bug的修复非常简单，也一并留作额外练习。
- 特别鸣谢Ximmbo da Jazz修复了一些错别字和错误的输出。
- 特别鸣谢Raghuprasad Govindarao发现了损坏的链接。
  
---
请将评论，建议和错误报告发送给[Andrew Gilpin](mailto:gilpin@cs.cmu.edu)。  
页面最后修改时间：2004年4月7日