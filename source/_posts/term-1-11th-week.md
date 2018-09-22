---
title: 'term 1: 11th week'
date: 2017-11-14 09:17:49
tags:
  - c++
  - vim
---

## 1114

### 读取文件失败原因总结

1. 文件名称打错，例如中文错别字，英文字母顺序写法、单词拼错。

2. 文件路径错误，例如相对路径错误，平台不同路径表示方法不同。

3. IDE中显示在同一目录下并不意味着文件的实际位置也位于同一目录下。

   {% asset_img text.png %}

   {% asset_img conf.png %}

   ​

## 1115

### LPCTSTR

> LPCTSTR用来表示你的字符是否使用UNICODE, 如果你的程序定义了UNICODE或者其他相关的宏，那么这个字符或者字符串将被作为UNICODE字符串，否则就是标准的ANSI字符串。
>
> **类型理解**
>
> LPCTSTR类型：
> L表示long指针 这是为了兼容Windows 3.1等16位操作系统遗留下来的，在win32中以及其他的32位操作系统中， long指针和near指针及far修饰符都是为了兼容的作用。没有实际意义。
> P表示这是一个指针
> C表示是一个常量
> T表示在Win32环境中， 有一个_T宏
> STR表示这个变量是一个字符串

### 宏定义中的`#`

在github上看到一段[代码](https://github.com/ryanofsky/cfree/blob/f3cdb4d9d179f3e803d5928ce64838f4fe34f850/test.cpp)，使用了一个宏定义，感觉蛮有意思，研究一下它干了些什么。

```c++
#include <iostream>

using namespace::std;

#define EXPR(x)  cout << #x << " = " << (x) << endl

int main(int, char *[])
{
  EXPR(2342 >> 2 && 2);
  EXPR((2342 >> 2) && 2);
  EXPR(2342 >> (2 && 2));
}
```

output:

```
2342 >> 2 && 2 = 1
(2342 >> 2) && 2 = 1
2342 >> (2 && 2) = 1171
```

根据输出结果可以看出来：

`#x`是将x表示的内容转换成字符串，`(x)`是x执行后的结果。

**Todo: **宏定义中的`#, ##, @#, /`符号改天专门整理一下。

## 1117

### vim替换反斜杠

由于C语言需要把路径中的`\`写成`\\`，使用替换命令`:s/\\/\\\\/g`时需要进行转义替换。