---
title: 'term 1: 13th week'
date: 2017-11-27 21:52:05
tags:
  - linux
---

## 1127

### Linux 下的输入输出和错误流

stdin     标准输入流，默认键盘
stdout  标准输出流，默认屏幕
stderr   标准错误流，错误输出

```c
#include <stdio.h>

int main()
{
  // printf("input a: ");
  fprintf(stdout, "input a: ");
  // scanf("%d", &a);
  fscanf(stdin, "%d", &a);
  if (a <= 0)
  {
    // stderr
    fprintf(stderr, "a must larger than 0\n");
    return 1; // a return value is necessary
  }
  return 0;
}
```

linux下可以使用`>, >>, <, <<`进行流的重定向，`<, >`会覆盖文件，`<<, >>`追加写入文件。

```c
// filaname: main.c
int main(int argc, const char *argv[])
{
  int a, b;

  printf("input a: ");
  scanf("%d", &a);
  printf("input b: ");
  scanf("%d", &b);

  if (0 == b) // can avoid the fault caused by misusing '=' and '=='
  {
    fprintf(stderr, "b can't equal to 0.");
    return 1;
  }
  printf("%d / %d = %d", a, b, a / b);

  return 0;
}
```

使用流重定向输出：

```sh
gcc main.c
./a.out 1> t.txt 2> f.txt
2
0
cat t.txt
# input a: input b: 
cat f.txt
# b can't equal to 0.
echo $?
# 1
```

管道功能示例：

```sh
ls
# a.out  flow.txt  f.txt  input.txt  main.c  t.txt
ls | grep txt
# flow.txt
# f.txt
# input.txt
# t.txt
```

```c
// filename: avg.c
#include <stdio.h>

int main(int argc, const char *argv[])
{
  int sum;
  float n;
  scanf("%d,%f", &sum, &n);
  printf("%f", sum / n);

  return 0;
}

// filename: input.c
#include <stdio.h>

int main(int argc, const char *argv[])
{
  int flag = 1;
  int n = 0;
  int sum = 0;
  while(flag)
  {
    scanf("%d", &flag);
    if (!flag)
    {
      break;
    }
    sum += flag;
    n++;
  }
  printf("%d,%d",sum,n);

  return 0;
}

/*运行
./input.out | ./avg.out
100
200
300
0
200.000000
*/

```

## 1129

### python 在Ubuntu下后台运行

> 使用 screen 很方便，有以下几个常用选项：
>
> - 用`screen -dmS *session name*`来建立一个处于断开模式下的会话（并指定其会话名）。
> - 用`screen -list `来列出所有会话。
> - 用`screen -r *session name*`来重新连接指定会话。
> - 用快捷键`CTRL-a d `来暂时断开当前会话。

引用自 [Linux 技巧：让进程在后台可靠运行的几种方法](https://www.ibm.com/developerworks/cn/linux/l-cn-nohup/index.html)

### Java虚拟机堆内存配置

使用Gradle编译Freenet时出现了如下错误：

[getting-gradle-error-could-not-reserve-enough-space-for-object-heap-constantly](https://stackoverflow.com/questions/26143740/getting-gradle-error-could-not-reserve-enough-space-for-object-heap-constantly)

感谢Stack Overflow...解决方法是在环境变量中进行如下设置：

> ```
> _JAVA_OPTIONS=-Xmx512M
> ```
>
> 1. Right click on start-button and open "System"
> 2. Go to Advanced system settings on the left side
> 3. Click the button "Environment Variables ..."
> 4. In System Variables, click "New..."
> 5. New Variable Name: _JAVA_OPTIONS
> 6. New Variable Value: -Xmx512M
> 7. Click OK
> 8. Restart Visual Studio, so the variable is picked up



## 1130

### JAVA泛型小记

> `?, T，E，K，V`这些全都属于java泛型的通配符，这些通配符其实**其实没什么区别**，只不过是一个约定好的代码，也就是说：
>
> 使用大写字母A,B,C,D......X,Y,Z定义的，就都是泛型，把T换成A也一样，这里T只是名字上的意义而已
>
> **?** 表示不确定的java类型
>
> **T (type)** 表示具体的一个java类型
>
> **K V (key value)**分别代表java键值中的Key Value
>
> **E** 代表Element
>
> 作者：程序鱼
> 链接：http://www.jianshu.com/p/95f349258afb
> 來源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 1202

### win7系统下使用vs 2017编译boost 1.65.1

1.启动vs 2017命令行工具。

2.切换目录到boost根目录下（有bootstrap.bat的那个目录）。

3.执行`bootstrap.bat` 。

4.执行.\b2 。

```sh
C:\local\boost165_1>bootstrap.bat

Building Boost.Build engine

Bootstrapping is done. To build, run:

    .\b2

To adjust configuration, edit 'project-config.jam'.

Further information:

    - Command line help:
    .\b2 --help
    
    - Getting started guide:
    http://boost.org/more/getting_started/windows.html
    
    - Boost.Build documentation:
    http://www.boost.org/build/doc/html/index.html

C:\local\boost165_1>.\b2

...... // compile progress

The Boost C++ Libraries were successfully built!

The following directory should be added to compiler include paths:

    C:\local\boost_1_65_1

The following directory should be added to linker library paths:

    C:\local\boost_1_65_1\stage\lib

```

## 1203

### c++ 下实现并使用split方法

参见[C++常见问题: 字符串分割函数 split](http://www.cnblogs.com/dfcao/p/cpp-FAQ-split.html)。

