---
title: 'term 1: 9th week'
tags:
  - c++
  - python
  - English
  - vim
date: 2017-10-31 19:55:33
---

第一学期第 8 周学习笔记。

<!-- more -->

## 1031

### c++中迭代器的种类

`input iterator` ：只能从头至尾遍历一次，因为在遍历过程中，指针的指向可能会变化，只用于读取容器中的数据，通常用于只需要遍历一次的、只读算法。

`output iterator`： 同样也只能从头至尾遍历一次，可以在不读取数据的情况下直接写入数据，通常用于只遍历一次的只写算法。

`forward iterator`： 遍历过程中不改变指针指向，可以用于多次的顺序遍历，支持读写数据。

`bidirectional iterator`： 拥有**forward iterator**迭代器的全部功能，支持双向遍历。

`random access iterator`：拥有**bidirectional iterator**迭代器的全部功能，支持随机访问，适用于排序、二分查找等需要随机存取的算法。

### python 使用 pickle 序列化对象

之前一直想使用 `pickle` 存取python对象，今天写了个脚本恰好需要使用，就借着这个机会简单记录一下其使用方法：

```python
import pickle

# 创建一个字典
words = {"hello":5, "world":5, "python":6, "c++":3}
# 使用pickle序列化words并存储
with open("wd.pickle", "wb") as f:
  # 第一个参数是需要序列化的对象，第二个参数是序列化对象存储的文件
  pickle.dump(words, f)
  # 脚本执行后会在其所在文件夹建立wd.pickle文件，使用cat 命令查看：
  # output:
  '''
  (dp0
  S'python'
  p1
  I6
  sS'world'
  p2
  I5
  sS'hello'
  p3
  I5
  sS'c++'
  p4
  I3
  s.r
  '''
  # 还可以使用第三个参数选择加密算法（可选）
  # 例如：
  # pickle.dump(words, f, 1)
  # cat wd.pickle
  # }q(UaqKUbqKu.
# 使用pickle读取文件并将其内容转化为python对象
with open('wd.pickle', 'rb') as f:
  data = pickle.load(f)
print data
# output:
# {'world': 5, 'c++': 3, 'hello': 5, 'python': 6}
```

### 迭代器继承

根据上面介绍的5中迭代器，可以看出来它们之间是有继承关系的。各个迭代器功能如下表所示(i for iterator, n is an integer)：

|     iterator capacity      | input iterator | output iterator | forward iterator | bidirectional iterator | random access iterator |
| :------------------------: | :------------: | :-------------: | :--------------: | :--------------------: | :--------------------: |
|     Dereferencing read     |      Yes       |       No        |       Yes        |          Yes           |          Yes           |
|    Dereferencing write     |       No       |       Yes       |       Yes        |          Yes           |          Yes           |
| Fixed and repeatable order |       No       |       No        |       Yes        |          Yes           |          Yes           |
|         i++    ++i         |      Yes       |       Yes       |       Yes        |          Yes           |          Yes           |
|         i--    --i         |       No       |       No        |        No        |          Yes           |          Yes           |
|            i[n]            |       No       |       No        |        No        |           No           |          Yes           |
|           i + n            |       No       |       No        |        No        |           No           |          Yes           |
|           i - n            |       No       |       No        |        No        |           No           |          Yes           |
|           i += n           |       No       |       No        |        No        |           No           |          Yes           |
|           i -= n           |       No       |       No        |        No        |           No           |          Yes           |



## 1101

### 一个关于 vector 构造函数的坑

c++ primer 书中介绍`copy()`函数时举了下面这个例子：

```c++
int casts[10] = {6, 7, 2, 9 ,4 , 11, 8, 7, 10, 5};
vector<int> dice[10];
copy(casts, casts + 10, dice.begin()); // copy array to vector
```

在Ubuntu环境下编译该代码出现如下错误：

`request for member which is of non-class type`

原因是 vector 初始化的问题，vector 应该使用构造函数初始化：

```c++
// no argument
vector<int> dice;
// has an argument to define the size of the vector
vector<int> dice(10);
```

再进行编译就没有个这个错误了。

完整代码：

```c++
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;


int main(int argc, const char *argv[])
{
    int casts[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
    vector<int> dice(10);
    // The first two iterator arguments to copy() represent a
    // range to be copied, and the final iterator argument 
    // represents the location to which the first item is copied.
    copy(casts, casts + 10, dice.begin());
    vector<int>::iterator it;
    it = dice.begin();
    for(;it != dice.end();it++)
       cout << *it << endl;
    return 0;
}
```

### copy(), ostream_iterator, istream_iterator

```c++
/***********************************************
 *
 *      Filename: copyit.cpp
 *
 *        Author: xss - callmexss@126.com
 *   Description: copy(), iostream, reverse iterator
 *        Create: 2017-11-01 22:07:55
 * Last Modified: 2017-11-01 22:45:13
 *
 ***********************************************/

#include <iostream>
#include <iterator>
#include <vector>


int main(int argc, const char *argv[])
{
    int cast[10] = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
    std::vector<int> dice(10);
    // copy from array to vector
    copy(cast, cast+10, dice.begin());
    std::ostream_iterator<int, char> out_iter(std::cout, " ");
    copy(dice.begin(), dice.end(), out_iter);
    std::cout << std::endl;
    // reverse it
    std::cout << "reverse implicitly: ";
    copy(dice.rbegin(), dice.rend(), out_iter);
    std::cout << std::endl;
    std::cout << "reverse explicitly: ";
    std::vector<int>::reverse_iterator ri;
    for(ri = dice.rbegin(); ri != dice.rend(); ri++)
        std::cout << *ri << ' ';
    std::cout << std::endl;
    std::vector<int> special(3);

    /* 未知错误...
    copy(std::istream_iterator<int, char>(std::cin),
         std::istream_iterator<int, char>(), special);
    cout << "before reverse: ";
    copy(special.begin(), special.end(), out_iter);
    cout << endl;
    cout << "after reverse: ";
    copy(special.rbegin(), special.rend(), out_iter);
    */
    return 0;
}
```



```c++
#include <iterator>
...
ostream_iterator<int, char> out_iter(cout, " ");

// You could use the iterator like this
*out_iter++ = 15; // works like cout << 15 << “ “;
```

> The out_iter iterator now becomes an interface that allows you to use cout to display information. The first template argument (int, in this case) indicates the data type being sent to the output stream. The second template argument (char, in this case) indicates the character type used by the output stream. (Another possible value would be wchar_t.) The first constructor argument (cout, in this case) identifies the output stream being used. It could also be a stream used for file output. The final character string argument is a separator to be displayed after each item sent to the output stream.

## 1102

### 插入迭代器

```c++
/***********************************************
 *
 *      Filename: insert.cpp
 *
 *        Author: xss - callmexss@126.com
 *   Description: insert iterator
 *        Create: 2017-11-02 20:48:44
 * Last Modified: 2017-11-02 21:03:20
 *
 ***********************************************/

#include <iostream>
#include <string>
#include <vector>
#include <iterator>

int main(int argc, const char *argv[])
{
    using namespace std;
    string s1[] = {"python", "c++", "java", "c#", "html"};
    string s2[] = {"network", "system", "automatic"};
    string s3[] = {"Tom", "Jerry"};
    // 如果有拷贝操作，一定要先使用构造函数初始化，否则会Segmentation fault
    // that is, if:
    // vector<string> words;
    // copy(s1, s1+5, words.begin());
    // than:
    // Segmentation fault
    vector<string> words(5); 
    copy(s1, s1+5, words.begin());
    ostream_iterator<string, char> out(cout, " ");
    cout << "init: ";
    copy(words.begin(), words.end(), out);
    cout << endl;
    cout << "back_insert: ";
    // there must be writen like this: 
    // back_insert_iterator<vector<T> > (something));
    copy(s2, s2+3, back_insert_iterator<vector<string> > (words));
    copy(words.begin(), words.end(), out);
    cout << endl;
    cout << "insert: ";
    copy(s3, s3+2, insert_iterator<vector<string> >(words, words.begin()));
    copy(words.begin(), words.end(), out);
    cout << endl;
    return 0;
}

```

输出：

```
init: python c++ java c# html
back_insert: python c++ java c# html network system automatic
insert: Tom Jerry python c++ java c# html network system automatic
```



### 省略

> That means when a container expires, so do the data stored in the container. (However, if the data is pointers, the pointed-to data does not necessarily expire.)

so后面的部分省略了动词*expires*，整句话的翻译为：

这意味伴随着一个容器的终止，容器中存储的数据也随之终止了。(然而如果其中的数据是指针，这些指针指向的数据不必被终止。)

### STL 容器

STL容器：`list, quene, deque, priority_quene, stack, vector, map, multimap, set, multiset, biset `

>  In the table, X represents a container type, such as vector, T represents the type of object stored in the container, a and b represent values of type X, and u represents an identifier of type X. 

| Expression    | Return Type              | Description                              | Complexity   |
| :------------ | ------------------------ | ---------------------------------------- | ------------ |
| X::iterator   | Iterator type point to T | Any iterator category except                                               output iterator | Compile time |
| X::value_type | T                        | The type for **T**                       | Compile time |
| X u;          |                          | Creates 0-size container called **u**    | Constant     |
| X();          |                          | Creates 0-size anonymous container       | Constant     |
| X u(a);       |                          | Copy constructor                         | Linear       |
| X u = a;      |                          | Same effect as **X u(a);**               |              |
| (&a)->~X();   | void                     | Applies destructor to every                                           element of a container | Linear       |
| a.begin()     | iterator                 | Returns an iterator referring                                                to the first element of  the container | Constant     |
| a.end()       | iterator                 | Returns an iterator that                                                         is a past-the-end value | Constant     |
| a.size()      | unsigned integral  type  | Returns number of elements                                        equal to **a.end()** - **a.begin()** | Constant     |
| a.swap(b)     | void                     | Swaps contents of a and b                | Constant     |
| a == b        | convertible to bool      | Returns **true** if a and b                                                     have the same size and each                                       element in a is equivalent to                                              (== is true) the corresponding                                   element in b | Linear       |
| a != b        | convertible to bool      | Returns !(a == b)                        | Linear       |

## 1103

### vim 常用技巧

[vim替换操作](http://54rd.net/html/2015/shell_0104/10.html)

### c++ namespace 问题

写了一个函数，用来输出列表元素：

```c++
void show(list<int> &li, string name)
{
    ostream_iterator<int, char> out(cout, " ");
    cout << "list " + name + ": ";
    copy(li.begin(), li.end(), out);
    cout << endl;
}
```

编译出现如下错误：

`variable or field declared void`

万能的**Stack Overflow**告诉我是因为没有加`using namespace std;`

>  [C++ Standard library classes are within the namespace `std::`.](https://stackoverflow.com/questions/364209/variable-or-field-declared-void)

### STL: list

```c++
/***********************************************
 *
 *      Filename: onest.cpp
 *
 *        Author: xss - callmexss@126.com
 *   Description: some list methods
 *        Create: 2017-11-03 10:44:30
 * Last Modified: 2017-11-03 12:03:54
 *
 ***********************************************/

#include <iostream>
#include <list>
#include <iterator>
#include <string>

using namespace std;

void show(list<int> &li, string name)
{
    ostream_iterator<int, char> out(cout, " ");
    cout << "list " + name + ": ";
    copy(li.begin(), li.end(), out);
    cout << endl;
}

void println(string s)
{
    cout << s << endl;
}

int main(int argc, const char *argv[])
{
    int num[5] = {1, 2, 3, 4, 5};
    list<int> one(5, 2); // this means 2 2 2 2 2
    list<int> two(3, 1);
    two.insert(two.begin(), num, num+5);
    list<int> three(two);
    int more[3] = {6, 7, 8};
    three.insert(three.end(), more, more+3);

    show(one, "one");
    show(two, "two");
    show(three, "three");

    three.remove(1);
    println("after remove 1:");
    show(three, "three");

    three.splice(three.begin(), one);
    println("list three after splice: ");
    show(three, "three");

    one.unique();
    println("after unique: ");
    show(one, "one");


    println("before operation on list two: ");
    show(two, "two");
    two.sort();
    two.unique();
    println("after sort&unique: ");
    show(two, "two");

    three.merge(two);
    println("after merge");
    show(two, "two");
    show(three, "three");
    println("unique it!");
    three.unique();
    show(three, "three");


    return 0;
}

```

## 1104

### SET

集合类定义在`set`中，集合中没有重复元素，集合有序。

```c++
/***********************************************
 *
 *      Filename: setops.cpp
 *
 *        Author: xss - callmexss@126.com
 *   Description: some set operations
 *        Create: 2017-11-04 18:23:37
 * Last Modified: 2017-11-04 20:00:16
 *
 ***********************************************/

#include <string>
#include <iostream>
#include <set>
#include <algorithm>
#include <iterator>

using namespace std;

void show(string str, const set<string> &s)
{
    ostream_iterator<string, char> out(cout, " ");
    cout << "set " << str << ": ";
    copy(s.begin(), s.end(), out);
    cout << endl;
}

int main(int argc, const char *argv[])
{
    const int N = 6;
    string s1[N] = {"buffoon", "thinkers", "for", "heavy", "can", "for"};
    string s2[N] = {"metal", "any", "food", "elegant", "deliver","for"};
    ostream_iterator<string, char> out(cout, " ");

    // init
    set<string> A(s1, s1+N);
    set<string> B(s2, s2+N);

    // show set elements
    show("A", A);
    show("B", B);

    // union
    cout << "Union of A and B:\n";
    set_union(A.begin(), A.end(), B.begin(), B.end(), out);
    cout << endl;

    // Intersection
    cout << "Intersection of A and B:\n";
    set_intersection(A.begin(), A.end(), B.begin(), B.end(), out);
    cout << endl;

    // Difference
    cout << "Difference of A and B:\n";
    set_difference(A.begin(), A.end(), B.begin(), B.end(), out);
    cout << endl;

    set<string> C;
    set_union(A.begin(), A.end(), B.begin(), B.end(), insert_iterator<set<string> >(C,           C.begin()));
    show("C", C);

    string s3("grungy");
    C.insert(s3);
    cout << "after insection, ";
    show("C", C);

    C.insert("super");

    cout << "Showing a range:\n";
    // [g*, s*)
    copy(C.lower_bound("ghost"), C.upper_bound("spook"), out);
    cout << endl;

    return 0;
}

```

输出如下：

```
set A: buffoon can for heavy thinkers
set B: any deliver elegant food for metal
Union of A and B:
any buffoon can deliver elegant food for heavy metal thinkers
Intersection of A and B:
for
Difference of A and B:
buffoon can heavy thinkers
set C: any buffoon can deliver elegant food for heavy metal thinkers
after insection, set C: any buffoon can deliver elegant food for grungy heavy metal thinkers
Showing a range:
grungy heavy metal
```

### multimap

`set, map`中键值唯一，`multiset, multimap`中一个键可以对应多个值。

```c++
/***********************************************
 *
 *      Filename: multimap.cpp
 *
 *        Author: xss - callmexss@126.com
 *   Description: use a multimap
 *        Create: 2017-11-04 20:20:42
 * Last Modified: 2017-11-04 21:46:47
 *
 ***********************************************/

#include <map>
#include <iostream>
#include <string>
#include <algorithm>


typedef int KeyType;
typedef std::pair<const KeyType, std::string> Pair;
typedef std::multimap<KeyType, std::string> MapCode;

void show(int code, std::multimap<KeyType, std::string> & m)
{
    std::cout << "Number of cities with area code " << code << ": "
         << m.count(code) << std::endl;
}

int main(int argc, const char *argv[])
{
    using namespace std;
    MapCode codes;

    codes.insert(Pair(415, "San Francisco"));
    codes.insert(Pair(510, "Oakland"));
    codes.insert(Pair(718, "Brooklyn"));
    codes.insert(Pair(718, "Staten Island"));
    codes.insert(Pair(415, "San Rafael"));
    codes.insert(Pair(510, "Berkely"));

    int code_num[4] = {415, 510, 571, 718};
    for (int i = 0; i < 4; i++)
    {
        show(code_num[i], codes);
    }

    // first, second
    cout << "Area Code City\n";
    MapCode::iterator it;
    for (it = codes.begin(); it != codes.end(); ++it)
        cout << "    " << (*it).first << "   " << (*it).second << endl;

    // range
    pair<MapCode::iterator, MapCode::iterator> range = codes.equal_range(718);
    cout << "Cities with area code 718:\n";
    for (it = range.first; it != range.second; ++it)
        cout << (*it).second << endl;

    return 0;
}

```

