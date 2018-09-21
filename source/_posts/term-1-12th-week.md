---
title: 'term 1: 12th week'
date: 2017-11-20 21:07:12
tags:
  - hexo
  - c++
  - linux
---

## 1120

### hexo 图片加载不正常

一个来自简书的[解决方法](http://www.jianshu.com/p/c2ba9533088a) 。

该方法使用的是`hexo-asset-img`插件，图片的插入方法不再使用*markdown*的语法，而是：

```yaml
{% asset_img example.png plus title %}
```
## 1121

### c++ 基本概念

> 一个 C++ 程序由含有[声明](http://zh.cppreference.com/w/cpp/language/declarations)的文本文件序列（通常为头文件与源文件）组成。它们被[翻译](http://zh.cppreference.com/w/cpp/language/translation_phases)成一个可执行文件，操作系统通过调用其 [main 函数](http://zh.cppreference.com/w/cpp/language/main_function)执行这一程序。

```c++
// hello
#include <iostream>
#include <string>
using namespace std;

// function declaration
void println(string s);
```

```c++
// hello.cpp
#include "hello.h"
int main()
{
	string s = "hellowrold";
	println(s);
	return 0;
}

// function implementation
void println(string s)
{
	cout << s << endl;
}

```



---

> 在 C++ 程序中，一些被称为[关键词](http://zh.cppreference.com/w/cpp/keyword)的词语有着特殊的含义。其它词语可以被用作[标识符](http://zh.cppreference.com/w/cpp/language/identifiers)。在翻译的过程中，[注释](http://zh.cppreference.com/w/cpp/comment)会被忽略。程序中的某些字符必须通过[转义序列](http://zh.cppreference.com/w/cpp/language/escape)表示。

 {% asset_img c++关键字.png c++ 关键字 %}

---



> C++ 程序中的**实体**包括值、[对象](http://zh.cppreference.com/w/cpp/language/object)、[引用](http://zh.cppreference.com/w/cpp/language/reference)、 [结构化绑定](http://zh.cppreference.com/w/cpp/language/structured_binding) (C++17 起)、[函数](http://zh.cppreference.com/w/cpp/language/functions)、[枚举项](http://zh.cppreference.com/w/cpp/language/enum)、[类型](http://zh.cppreference.com/w/cpp/language/type)、类成员、[模板](http://zh.cppreference.com/w/cpp/language/templates)、[模板特化](http://zh.cppreference.com/w/cpp/language/template_specialization)、[命名空间](http://zh.cppreference.com/w/cpp/language/namespace)、[形参包](http://zh.cppreference.com/w/cpp/language/parameter_pack)和 this 指针。预处理器[宏](http://zh.cppreference.com/w/cpp/preprocessor/replace)不是 C++ 实体。



### c++ 输入、输出和文件

#### 流和缓冲区

 {% asset_img std-io-complete-inheritance.svg 基于流的 I/O 继承图 %}

C++程序将输入和输出视为字节流。输入过程中，程序从输入流中抽取字节；输出过程中，程序将字节插入到输出流中。C++在输入和输出过程只关心字节流而不关心其来源和去向。

 {% asset_img ioexample.png C++的输入和输出 %}

通常情况下，使用缓冲区可以更高效处理输入和输出。**缓冲区**是一个内存块，作为程序和设备的中间件，用来存储在程序和设备之间流转的数据。

#### cout

 {% asset_img cout.png cout的输出过程 %}

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>
#include <cstring>
// review the iterator
void show(vector<char> & v)
{
	ostream_iterator<char, char> out(cout);
	copy(v.begin(), v.end(), out);
	cout << endl;
}


int main()
{
    // review the vector
	vector<char> charlist = { 'a', 'p', 'p', 'l', 'e' };
	charlist.push_back('.');
	
	show(charlist);

    // ust the put function, which displays a char
	for (int i = 0; i < charlist.size(); i++)
	{
		cout << i << ": ";
		cout.put(charlist[i]);
		cout << "\n";

	}

	const char * state1 = "Lfish";
	const char * state2 = "theoceanside";
	int len = strlen(state1);
	int i = 1;

    // use the write function, which displays a string by length
	cout << "Increase loop index:\n";
	for (i = 1; i <= len; i++)
	{
		cout.write(state1, i);
		cout << endl;
	}

	cout << "Decrease loop index:\n";
	for (i = len; i > 0; i--)
		cout.write(state1, i) << endl;

	// exceed the string length
	cout << "exceed the string length: ";
	cout.write(state1, len + 5) << endl;
  
	return 0;
	
}
```

output:

```
apple.
0: a
1: p
2: p
3: l
4: e
5: .
W
Increase loop index:
L
Lf
Lfi
Lfis
Lfish
Decrease loop index:
Lfish
Lfis
Lfi
Lf
L
exceed the string length: Lfishaaath
```

cout 格式化输出：

```c++
// some include, define, usring...
int main()
{
    // flush 操作用于刷新缓冲区。
    // 向屏幕输出时，不必等到缓冲区填满才输出。
    // 向缓冲区输入换行符或者等待输入时都会刷新缓冲区。
    // cout << endl; 操作等同于 cout << flush << "\n";
    // cout << flush; 等同于 flush(cout);
    
    // 可以用以下方法输出8进制和16进制数。
	int n = 10;
    // 以8进制输出 10
	oct(cout);
	cout << n << endl;
    // 以16进制输出 a
	cout << hex << n << endl;
    

    // '' -> 字符，（本质是整型，多个字符会计算其乘积作为结果）
    // "" -> 字符串
    // cout << 'a: '; ---> 613a20
	cout << "a: ";
  
    // 设置宽度
    cout.width(12);
    cout << "apple" << endl;
    cout << flush << '\n';
  
    dec(cout);
    int w = cout.width(30);
    cout << "default field width = " << w << ":\n";
    cout.width(5);
    cout << "N" << ':';
    cout.width(8);
    cout << "N * N" << ":\n";
    for (long i = 1; i <= 100; i *= 10)
    {
        cout.width(5);
        cout << i << ':';
        cout.width(8);
        cout << i * i << ":\n";
    }
  
    // 设置填充字符
    cout.fill('*');
    const char * staff[2] = { "Waldo Whipsnade", "Wilmarie Wooper" };
    long bonus[2] = { 900, 1350 };
    for (int i = 0; i < 2; i++)
    {
        cout << staff[i] << ": $";
        cout.width(7);
        cout << bonus[i] << "\n";
    }
    cout << endl;
  
    // 设置浮点数精度
    // 四舍五入
    float pi = 3.14159;
    cout << pi << endl;
    cout.precision(5);
    cout << pi << endl;
    cout << endl;

    // setf --> set flag
    cout.setf(ios_base::showpoint);
    float ten = 10.;
    float twenty0four = 20.04;
    cout.precision(4);
    cout << ten << endl;
    cout << twenty0four << endl;
  
    return 0;
}
```

output:

```
12
a
a:        apple

        default field width = 0:
    N:   N * N:
    1:       1:
   10:     100:
  100:   10000:
Waldo Whipsnade: $****900
Wilmarie Wooper: $***1350

3.14159
3.1416

10.00
20.04
```

格式化常量：

| Constant            | Meaning                                  |
| ------------------- | ---------------------------------------- |
| ios_base::boolalpha | Input and output bool values as true and false |
| ios_base::showbase  | Use C++ base prefixes (0,0x) on output   |
| ios_base::showpoint | Show trailing decimal point              |
| ios_base::uppercase | Use uppercase letters for hex output, E notation |
| ios_base::showpos   | Use + before positive numbers            |

## 1122

### tar——压缩、解压缩命令

> 压　缩：tar -jcv -f filename.tar.bz2 要被压缩的文件或目录名称
> 查　询：tar -jtv -f filename.tar.bz2
> 解压缩：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录
> ————tar -zxvf 路径/文件名.tar.gz
> 来自: <http://man.linuxde.net/tar>

## 1123

### 预处理器

#### \#if - #ifdef - #ifndef

> 预处理器支持有条件地编译部分源文件。这一行为由 `#if`、`#else`、`#elif`、`#ifdef`、`#ifndef` 与 `#endif` 指令所控制。

| 语法            |
| ------------- |
| `#if` 表达式     |
| `#ifdef` 表达式  |
| `#ifndef` 表达式 |
| `#elif` 表达式   |
| `#else`       |
| `#endif`      |

> **解释**
> 条件编译预处理块由 `#if`、`#ifdef` 或 `#ifndef` 指令开始，并可选地包含任意多个 `#elif` 指令，接下来是至多一个可选的 `#else` 指令，并以 `#endif` 指令结束。嵌套的条件编译区块会被单独处理。
>
> 各个 `#if`、`#elif`、`#else`、`#ifdef` 和 `#ifndef` 指令所控制的代码块在第一个不属于内部嵌套的条件编译区块的 `#elif`、`#else` 或 `#endif` 指令处结束。
>
> `#if`、`#ifdef` 和 `#ifndef` 指令测试其所指定的条件（见下文），如果条件求值为真，则编译其控制的代码块。此时后续的 `#else` 和 `#elif` 指令将被忽略。否则，如果所指定的条件求值为假，则将跳过其所控制的代码块，然后处理后续的 `#else` 或 `#elif` 指令（如果存在）。前一种情况下，`#else` 指令所控制的代码块将会被无条件地进行编译。后一种情况下，`#elif` 指令按照与 `#if` 指令相同的方式执行：即测试条件是否满足，并根据其结果决定编译或跳过其所控制的代码块，后一种情况下继续处理后续的 `#elif` 和 `#else` 指令。条件编译区块以 `#endif` 指令结束。

例子：

```c++
#define ABCD 2
#include <iostream>
 
int main()
{
 
#ifdef ABCD
    std::cout << "1: yes\n";
#else
    std::cout << "1: no\n";
#endif
 
#ifndef ABCD
    std::cout << "2: no1\n";
#elif ABCD == 2
    std::cout << "2: yes\n";
#else
    std::cout << "2: no2\n";
#endif
 
#if !defined(DCBA) && (ABCD < 2*4-3)
    std::cout << "3: yes\n";
#endif
}
```



#### \#define - # - ## - #include

\#define

> 预处理器支持文本宏替换。亦支持仿函数文本宏替换。
| 语法                            |
| ----------------------------- |
| `#define` 标识符 替换列表(可选)        |
| ` #define` 标识符( 形参 ) 替换列表     |
| `#define` 标识符( 形参, ... ) 替换列表 |
| `#define` 标识符( ... ) 替换列表     |
| `#undef` 标识符                  |

---
\# 和 \##
> 仿函数宏中， 替换列表 中标识符前的 `#` 运算符经通过标识符运行形参替换，并将结果以引号环绕，等效地创建字符串字面量。另外，预处理器添加反斜杠以转义环绕内嵌字符串字面量的引号，若它存在，并按需要双写字符串中的反斜杠。移除所有前导和尾随空白符，并将文本中部（但非内嵌字符串字面量中内）的任何空白符序列缩减成单个空格。此操作被称为“字符串化”，若字符串化的结果不是合法的字符串字面量，则行为未定义。

```c++
// # 出现于 __VA_ARGS__ 前时，整个展开的 __VA_ARGS__ 被包在引号中：
#define showlist(...) puts(#__VA_ARGS__)
showlist();            // 展开成 puts("")
showlist(1, "x", int); // 展开成 puts("1, \"x\", int")
```

> 替换列表 中任何二个相继标识符间的 `##` 运算符在二个标识符（首先未被宏展开）上运行形参替换，然后连接结果。此操作被称为“连接”或“记号粘贴”。只有一同组成合法记号的记号才可以粘贴：组成更长标识符的标识符、组成数字的数位，或组成 `+=` 的运算符 `+` 与 `=`。不能以粘贴 `/` 与 `*` 创建注释，因为考虑文本宏替换前，注释就被移除了。若连接的结果不是合法记号，则行为未定义。

例子：

```c++
#include <iostream>
 
// 制造函数工厂并使用它
#define FUNCTION(name, a) int fun_##name() { return a;}
 
FUNCTION(abcd, 12)
FUNCTION(fff, 2)
FUNCTION(qqq, 23)
 
#undef FUNCTION
#define FUNCTION 34
#define OUTPUT(a) std::cout << #a << '\n'
 
int main()
{
    std::cout << "abcd: " << fun_abcd() << '\n';
    std::cout << "fff: " << fun_fff() << '\n';
    std::cout << "qqq: " << fun_qqq() << '\n';
    std::cout << FUNCTION << '\n';
    OUTPUT(million);               // 注意缺少引号
}
```

---

\#include 

| 语法                                       |
| ---------------------------------------- |
| #include <文件名>                           |
| #include "文件名"                           |
| `__has_include ( " 文件名 " )`                                                                                                                                                                 `__has_include ( < 文件名 > )` |

> 任何预处理记号（宏常量或者表达式）都允许用作 `#include` 和 `__has_include` (C++17 起)的实参，只要它们扩展为以 < > 或者 " " 所包围的字符序列即可。



最近遇到一种头文件里的写法，一直没有查是什么意思，今天偶然看[谷歌C++风格指南](http://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/headers/#define-guard) 看到了答案。

> 头文件应该能够自给自足（self-contained,也就是可以作为第一个头文件被引入），以 `.h` 结尾。至于用来插入文本的文件，说到底它们并不是头文件，所以应以 `.inc` 结尾。不允许分离出 `-inl.h` 头文件的做法。



> 当包含一个文件时，将进行翻译阶段 1 到 4 的处理，其中将递归地包括对其中嵌套的 #include 指令的扩展。为了避免相同文件被重复包含，和当文件（可能传递性地）包含自身时的无限递归，通常会使用头文件防护，整个头文件被包含到：

```c++
// 为保证唯一性, 头文件的命名应该基于所在项目源代码树的全路径.
// 例如, 项目 foo 中的头文件 foo/src/bar/baz.h 可按如下方式保护:
#ifndef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_
...
#endif // FOO_BAR_BAZ_H_
```

即头文件需要**` #define` 保护**（所有头文件都应该使用 `#define` 来防止头文件被多重包含, 命名格式当是: `<PROJECT>_<PATH>_<FILE>_H_` .）。

#### \#error - #pragma - #line



参考 http://zh.cppreference.com/w/cpp/preprocessor

### Kali Linux persistence 配置

1. 使用UltraISO制作U盘镜像。

2. 进入BIOS选择U盘启动。

3. 在启动界面选择 Live USB Persistence)。

   {% asset_img kali_boot.png boot menu%}

4. 进入系统后打开终端输入gparted进行分区。

   1. 在gparted中选择U盘（它会识别系统中可以识别的所有硬盘，所以看仔细别把C盘干掉了...）。
   2. 选择合适的新分区大小，设置为**ext4**格式，创建新分区（U盘可以留一部分空间继续当U盘 :)...在Windows下面新建分区的部分不会再被识别，所以看到U盘容量变小了不要差异...）。
   3. 标签名称设置为persistence（别的俺没有试，不造管用不...）。
   4. 最后前往不要忘了保存所有设置，在菜单栏某一条里面（会进行分区操作之类的...稍等一会儿）。
   5. 挂载新分区并进行配置。

   ```sh
   mkdir -p /mnt/my_kali
   mount /dev/sdb2 /mnt/my_kali # replace 'sdb2' with the name of your partition. 
   echo "/ union" >> /mnt/my_kali/persistence.conf
   umount /mnt/usb
   ```

   然后创建一个文件，重启一下：

   ```sh
   touch test.txt
   reboot
   ```

然后如果重启以后没有看到这个文件...先别着急，打开gparted看一下配置是否还在，就是标签还是不是persistence之类的，如果还在，那就说明配置成功了，再创建一个文件试试看吧。Any way this works fine for me :) 

之后的一些配置：

修改一下sources.list可以加快软件的安装和更新速度：

  ```sh
  lsb_release -a # 下查看一下自己是什么版本

  vim /etc/apt/sources.list

  # sources.list
  #阿里云
  deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
  deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib

  #中科大
  #deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
  #deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib

  #清华大学
  #deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
  #deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free

  #浙大
  #deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
  #deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free


  ```

---

如果你是IPv6用户，可以改个hosts，然后你就可以用Google了：

  ```sh
  #!/usr/bin/bash

  mv /etc/hosts /etc/hosts.bak
  wget https://raw.githubusercontent.com/lennylxx/ipv6-hosts/master/hosts
  mv hosts /etc/

  /etc/init.d/networking restart
  ```

可以写个脚本执行一下，也可以单步执行。

---

如果你是python用户，可以改一下pip的默认仓库主页，加快包安装的速度：

  ```sh
  # 查看 ~ 目录下是否有 .pip 文件夹
  ls -a
  # 没有建一个
  mkdir .pip && cd .pip
  # 创建配置文件
  vim pip.config
  # pip.config
  [global]
  timeout = 60
  index-url = http://download.zope.org/ppix
  ```



## 1124

### vs2017 使用 Google Test

想要养成好的编码习惯，边写程序边写单元注释，找到了[**gtest**](https://github.com/google/googletest)框架，于是准备试用一下。结果就折腾了一下午，记一下这个大坑。

首先**What** ：

> gtest (Google Test) is a unit testing library for the C++ programming language, based on the xUnit architecture.

简单来说它就是一个和**Junit**类似的单元测试框架。

然后**How** ：

从官方仓库的**release**中下载压缩文件，解压后直接使用vs2017打开工程编译即可，编译完成后得到库文件`gtestd.lib` 。按照下面的方法配置到vs2017中：

1. 右键点击打开方案属性窗口，选择c/c++中的常规选项，在**附加包含目录**下填入解压缩后文件中的`include`文件夹路径，或者自己更改`include`文件夹的位置，如果想对路径搞不对，直接用绝对路径：

   {% asset_img c_c++_general.png  配置附加包目录 %}

2. 选择连接器中的输入选项，在**附加依赖项**中填写`gtestd.lib`的路径，相对路径写不对请直接用绝对路径：

   {% asset_img linker_input.png 配置附加依赖项 %}

   如果遇到`LNK1104`的错误，八成是路径没写对...然后就配置好了，编译一下。

3. 运行一下：

   [code](http://www.cnblogs.com/coderzh/archive/2009/03/31/1426758.html):

   ```c++
   #include "gtest/gtest.h"

   int Foo(int a, int b)
    {
      if (a == 0 || b == 0)
      {
        throw "don't do that";
      }
      int c = a % b;
      if (c == 0)
        return b;
      return Foo(b, c);
    }
    
    TEST(FooTest, HandleNoneZeroInput)
    {
      EXPECT_EQ(2, Foo(4, 10));
      EXPECT_EQ(6, Foo(30, 18));
     }

     int main(int argc, _TCHAR* argv[])
     {
       testing::InitGoogleTest(&argc, argv);
       return RUN_ALL_TESTS();
       system("pause");
         return 0;
     }
   ```


   {% asset_img test_res.png 测试结果 %}

**最后...**
   终于搞定...想哭啊！！！



## 1125

### c++ 互斥

c++如何实现互斥？来自[stackoverflow](https://stackoverflow.com/questions/34524/what-is-a-mutex)的一个答案：

> When I am having a big heated discussion at work, I use a rubber chicken which I keep in my desk for just such occasions. The person holding the chicken is the only person who is allowed to talk. If you don't hold the chicken you cannot speak. You can only indicate that you want the chicken and wait until you get it before you speak. Once you have finished speaking, you can hand the chicken back to the moderator who will hand it to the next person to speak. This ensures that people do not speak over each other, and also have their own space to talk.
>
> Replace Chicken with Mutex and person with thread and you basically have the concept of a mutex.
>
> Of course, before you use the rubber chicken, you need to ask yourself whether you actually need 5 people in one room and would it not just be easier with one person in the room on their own doing all the work. Actually, this is just extending the analogy, but you get the idea.

翻译之就是：

当我在工作中有一个很想讨论的话题的时候，这种时候我就会用到我一直放在桌上的橡皮鸡了。只有持有橡皮鸡的人才被允许讨论。如果你没有才由它，你就不能说话。你只能表明你需要这只鸡，在你得到这只鸡以前，你只能保持沉默。一旦你的讨论结束了，你需要把这只鸡还给管理者，然后他再将这只鸡发给下一个想要说话的人。这就保证了人们的讨论不会打扰到彼此（同一时间只有一个人在说话），同时他们也拥有独立的空间进行讨论。
把鸡替换成互斥锁，把人替换成线程然后你就得到了互斥的概念。
​当然，在你准备使用橡皮鸡的时候，你需要问一下你自己你是否真的需要五个人，一个人能不能来完成这整个工作。事实上，这只是扩展了这个比喻，但是你应该已经搞懂这个概念了。

然后实现的简单[代码](https://stackoverflow.com/questions/4989451/mutex-example-tutorial)：

```c++
// A thread is : Each person
// The mutex is : The door handle
// The lock is : The person's hand
// The resource is : The phone
#include <iostream>
#include <thread>
#include <mutex>

std::mutex m;//you can use std::lock_guard if you want to be exception safe
int i = 0;

void makeACallFromPhoneBooth() 
{
    m.lock();//man gets a hold of the phone booth door and locks it. The other men wait outside
      //man happily talks to his wife from now....
      std::cout << i << " Hello Wife" << std::endl;
      i++;//no other thread can access variable i until m.unlock() is called
      //...until now, with no interruption from other men
    m.unlock();//man lets go of the door handle and unlocks the door
}

int main() 
{
    //This is the main crowd of people uninterested in making a phone call

    //man1 leaves the crowd to go to the phone booth
    std::thread man1(makeACallFromPhoneBooth);
    //Although man2 appears to start second, there's a good chance he might
    //reach the phone booth before man1
    std::thread man2(makeACallFromPhoneBooth);
    //And hey, man3 also joined the race to the booth
    std::thread man3(makeACallFromPhoneBooth);

    man1.join();//man1 finished his phone call and joins the crowd
    man2.join();//man2 finished his phone call and joins the crowd
    man3.join();//man3 finished his phone call and joins the crowd
    return 0;
}

```



### 高质量C++/C编程指南附录试题笔记

**一、请填写BOOL , float, 指针变量 与“零值”比较的 if 语句。**

```c++
// BOOL, give `flag`
if (flag)
if (!flag)

// float, give `x`
const float EPSINON=0.00001;
if ((x >= -EPSINON) && (x <= EPSINON))
  
// pointer, give `cahr *p`
if (p == NULL)
if (p != NULL)
```



**二、const 有什么用途？（请至少说明两种）**

（1）可以定义 const 常量。

（2）const可以修饰函数的参数、返回值，甚至函数的定义体。被const修饰的东西都受到强制保护，可以预防意外的变动，能提高程序的健壮性。

**三、在C++ 程序中调用被 C编译器编译后的函数，为什么要加 extern “C”？**

C++语言支持函数重载，C语言不支持函数重载。函数被C++编译后在库中的名字与C语言的不同。假设某个函数的原型为： void foo(int x, int y);
该函数被C编译器编译后在库中的名字为\_foo，而C++编译器则会产生像\_foo\_int\_int之类的名字。
C++提供了C连接交换指定符号extern“C”来解决名字匹配问题。

## 1126

### c++ 编码规范小例

```c++
class Node // 类名大写，分割用的大括号均换行
{
 // public, private等访问控制符缩进 1 个空格
 // 以行为为中心，即public在前
 public:
   // 其余缩进均为 2 个空格
   Node(); // 函数和后面的'('之间不带空格分割
   int Start(string args); // 函数名每个单词的首字母大写
   float GetLoc();
   void SetLoc(float loc); // 具有相反功能的函数应使用一组反义词来定义 
   ...
 // 以数据为中心，即private在后
 private:
   float loc = 0.0; // 就近原则，最好定义完就初始化
   bool isStart = false; // 变量也是用驼峰命名法，但是开头字母小写
}
```

