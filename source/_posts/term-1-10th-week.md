---
title: 'term 1: 10th week'
tags:
  - c++
  - qt5
date: 2017-11-06 11:41:28
---

第一学期第 10 周学习笔记。

<!-- more -->

## 1106

### 函数对象（函子）

>  A functor is any object that can be used with () in the manner of a function. This includes normal function names, pointers to functions, and class objects for which the () operator is overloaded—that is, classes for which the peculiar-looking function operator()() is defined. For example, you could define a class like this: 

```c++
#include <iostream>

class Linear
{
    private:
        float slope; // 斜率
        float y0; // 函数值
    public:
        Linear(float _sp = 1, float _y = 0)
            : slope(_sp), y0(_y) {}
        double operator() (double x) { return x * slope + y0; }
};

int main()
{
    Linear f1;
    Linear f2(2.5, 10.0);
    double y1 = f1(12.4);
    double y2 = f2(0.4);
    std::cout << "y1: " << y1 << " y2: " << y2 << std::endl;
}
```

```c++
/***********************************************
 *
 *      Filename: functor.cpp
 *
 *        Author: xss - callmexss@126.com
 *   Description: functor technique
 *        Create: 2017-11-06 14:32:18
 * Last Modified: 2017-11-06 14:47:37
 *
 ***********************************************/

#include <iostream>
#include <list>
#include <iterator>


template<class T>   // functor class defines operator()()
class TooBig
{
    private:
        T cutoff;
    public:
        TooBig(const T & t) : cutoff(t) {}
        bool operator()(const T & v) { return v > cutoff; }
};

void show(const std::list<int> & li)
{
    std::ostream_iterator<int,char> out(std::cout, " ");
    copy(li.begin(), li.end(), out);
    std::cout << std::endl;
}

int main(int argc, const char *argv[])
{
    using std::list;
    using std::cout;
    using std::endl;

    TooBig<int> f100(100);
    list<int> yadayada;
    list<int> etcetera;
    int vals[10] = {50, 100, 90, 180, 60, 210, 415, 88, 188, 201};

    yadayada.insert(yadayada.begin(), vals, vals+10);
    etcetera.insert(etcetera.begin(), vals, vals+10);
    cout << "Original lists:\n";
    show(yadayada);
    show(etcetera);

    // use a named function object
    yadayada.remove_if(f100);
    // construct a function object
    etcetera.remove_if(TooBig<int>(200));

    cout << "Trimmed lists:\n";
    show(yadayada);
    show(etcetera);

    return 0;
}
```

output:

```
Original lists:
50 100 90 180 60 210 415 88 188 201
50 100 90 180 60 210 415 88 188 201
Trimmed lists:
50 100 90 60 88
50 100 90 180 60 88 188

```

> Just as the STL defines concepts for containers and iterators, it defines functor concepts:
> • A generator is a functor that can be called with no arguments.
> • A unary function is a functor that can be called with one argument.
> • A binary function is a functor that can be called with two arguments. 

```c++
// [sqrt(n) for n in gr8]
transform(gr8.begin(), gr8.end(), out, sqrt);
// map(lambda x, y: (x+y)/2, gr8, m8)
transform(gr8.begin(), gr8.end(), m8.begin(), out, mean);
```

### QT5 窗体图标不显示

在csdn上找到了[答案](http://blog.csdn.net/lucky_vip/article/details/22328215)。

原因是因为路径不正确，代码如下：

```c++
this->setWindowIcon(QIcon(":/Image/icon.png")) // for linux, if windows, do nothing
this->setWindowIcon(QIcon(":\\Image\\icon.png")); // show the icon successfully
```

### c++ 数字转字符串

```c++
#include <iostream>
#include <string>
#include <sstream>

int main(int argc, const char *argv[])
{
    std::ostringstream ss;
    int cheeses;
    std::cin >> cheeses;
    ss << cheeses;
    // std::string s;
    // s = std::to_string(cheeses);
    std::cout << "We have " + ss.str() + " varieties of cheese." << std::endl;
    return 0;
}
```

在so上面看到一个[简单的方法](https://stackoverflow.com/questions/5590381/easiest-way-to-convert-int-to-string-in-c)，但是在`ubuntu`下编译出现错误【`'to_string' is not a member of 'std'`】，这是g++编译器的一个著名[bug](https://stackoverflow.com/questions/12975341/to-string-is-not-a-member-of-std-says-g-mingw)。

以及这就是为啥python好用的原因了...

```python
cheeses = 32
# int to string
cheeses_s = string(cheeses)
# string to int
cheeses_n = int(cheeses_s)
```

## 1107

### c++ primer plus 5th 第三章复习题笔记

1.Consider the two C++ statements that follow:

```c++
char grade = 65;
char grade = 'A';
```

Are they equivalent? 

我以为它俩不相等，实际上是**相等**的。代码如下：

```c++
#include <iostream>
#include <string>

using namespace std;

int main(int argc, const char *argv[])
{
    char i1 = 65;
    char i2 = 'A';
    cout << i1 << " " << sizeof(i1) << endl;
    cout << i2 << " " << sizeof(i2) << endl;
    cout << (i1 == i2?'y':'n') << endl;
    return 0;
}
```

output:

```
A 1
A 1
y
```

看了书中的答案发现被打脸...

> The two statements are not really equivalent, although they have the same effect on some systems. Most importantly, the first statement assigns the letter A to grade only on a system using the ASCII code, while the second statement also works for other codes. Second, 65 is a type int constant, whereas ‘A’ is a type char constant. 

---

2.How could you use C++ to find out which character the code 88 represents? Come up with at least two ways.

```c++
// method 1
char c = 88;
cout << c << endl;
// method 2
cout.put(char(88));
cout << endl;
// method 3
cout << char(88) << endl;
// method 4
cout << (char) 88 << endl;
```

---

3.Assigning a long value to a float can result in a rounding error. What about assigning long to double?

书中的答案：

> The answer depends on how large the two types are. If long is 4 bytes, there is no loss.
> That’s because the largest long value would be about 2 billion, which is 10 digits.
> Because double provides at least 13 significant figures, no rounding would be needed.

测试代码：

```c++
#include <iostream>
#include <string>

using namespace std;

int main(int argc, const char *argv[])
{
    long i = 3141592653;
    float f = i;
    double d = i;
    cout << f << endl;
    cout << d << endl;
    return 0;
}
```

在*Ubuntu 16.04 32bit*下使用g++编译:

```
3.14159e+09
3.14159e+09
```

在*win7 64bit*下使用vs2017编译：

```
-1.15337e+09
-1.15337e+09
// change i to 314159265
3.14159e+08
3.14159e+08
```

因此具体是否会出现精度丢失应该视操作系统和编译器而定。

## 1109

### cout 格式化输出字符

```c++
/***********************************************
 *
 *      Filename: 3_1.cpp
 *
 *        Author: xss - callmexss@126.com
 *   Description: inches to feet and inches
 *        Create: 2017-11-07 10:58:49
 * Last Modified: 2017-11-07 16:23:34
 *
 ***********************************************/
#include <iostream>

using namespace std;

int main(int argc, const char *argv[])
{
    int height;
    const int InchesInFoot = 12;
    cout.setf(ios_base::fixed, ios_base::floatfield);
    cout << "Input your height(inches):___" << "\b\b\b";
    cin >> height;
    cout << "you're " << height / InchesInFoot << " feets, " << height % InchesInFoot <<" inches" << endl;
    return 0;
}
```

题目要求在下划线上进行输入，因此使用了`cout.setf()`控制输出格式。该方法有两种重载形式：

**std::ios_base::setf**

| set (1)      | `fmtflags setf (fmtflags fmtfl);`        |
| ------------ | ---------------------------------------- |
| **mask (2)** | **`fmtflags setf (fmtflags fmtfl, fmtflags mask);`** |

> Set specific format flags
>
> The first form *(1)* sets the stream's *format flags* whose bits are set in fmtfl, leaving unchanged the rest, as if a call to `flags(fmtfl|flags())`.
> The second form *(2)* sets the stream's *format flags* whose bits are set in both fmtfl and mask, and clears the *format flags* whose bits are set in mask but not in fmtfl, as if a call to `flags((fmtfl&mask)|(flags()&~mask))`.
> Both return the value of the stream's *format flags* before the call.
> The format flags of a stream affect the way data is interpreted in certain input functions and how it is written by certain output functions. See [ios_base::fmtflags](http://www.cplusplus.com/ios_base::fmtflags) for the possible values of this function's arguments.

## 1110

### c++ 数组初始化

只有在定义数组的时候才能使用形式：`tpye typeName[size]; or tpye typeName[size] = {}; `

```c++
int cards[4] = {3, 6, 8, 10}; // okay
int hand[4]; // okay
hand[4] = {5, 6, 7, 9}; // not allowed
hand = cards; // not allowed
```

初始化数组时，可以提供比数组元素更少的值。

```c++
float hotelTips[5] = {5.0, 2.5};
```

如果部分初始化一个数组，编译器会将剩余的元素设置为零。例如，想要生成一个全0的数组：

```c++
int allZero[10] = {0};
// all zero but the first one to be 1
int allZeroExceptFirst[10] = {1};
```

如果初始化数组时`[]`中没有填写数组大小，c++编译器会完成这个工作，它会统计数组中元素的个数。通常是不建议使用这种初始化方法的，但是初始化一个字符数组并将其转化为字符串是安全（后面会讲）。可以用以下代码测试编译器是否进行了这个工作。

```c++
#include <iostream>

using namespace std;

int main(int argc, const char *argv[])
{
    short things[] = {1,2,3,4};
    int num_elements = sizeof things / sizeof(short);
    cout.setf(ios_base::boolalpha);
    cout << (num_elements == 4) << endl;

    return 0;
}
// output is "true"
```

