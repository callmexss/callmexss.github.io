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

