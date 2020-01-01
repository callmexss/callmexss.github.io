---
title: 'term 1: 14th week'
date: 2017-12-07 11:31:34
tags:
  - java
  - c++
---

第一学期第 14 周学习笔记。

<!-- more -->

## 1207

### 【JAVA基础】HashSet、LinkedHashSet、TreeSet使用区别

[【JAVA基础】HashSet、LinkedHashSet、TreeSet使用区别](http://www.cnblogs.com/ibook360/archive/2011/11/28/2266062.html)

> **HashSet**：哈希表是通过使用称为散列法的机制来存储信息的，元素并没有以某种特定顺序来存放；
>
> **LinkedHashSet**：以元素插入的顺序来维护集合的链接表，允许以插入的顺序在集合中迭代；  
>
> **TreeSet**：提供一个使用树结构存储Set接口的实现，对象以升序顺序存储，访问和遍历的时间很快。

### c++ 模板类问题

c++ 模板类不分离，声明和定义均写在同一个头文件中。否者会有编译错误，据说这种编译是可以解决的，具体未查。

## 1208

### 抽象类

> **抽象类是**不完整的，它只能用作基**类**。 在面向对象方法中，**抽象类**主要用来进行类型隐藏和充当全局变量的角色。

**c++接口**

> 接口描述了类的行为和功能，而不需要完成类的特定实现。
>
> C++ 接口是使用**抽象类**来实现的，抽象类与数据抽象互不混淆，数据抽象是一个把实现细节与相关的数据分离开的概念。
>
> 如果类中至少有一个函数被声明为纯虚函数，则这个类就是抽象类。纯虚函数是通过在声明中使用 "= 0" 来指定的。

```c++
class Box
{
   public:
      // 纯虚函数
      virtual double getVolume() = 0;
   private:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
};
```

> 设计**抽象类**（通常称为 ABC）的目的，是为了给其他类提供一个可以继承的适当的基类。抽象类不能被用于实例化对象，它只能作为**接口**使用。如果试图实例化一个抽象类的对象，会导致编译错误。
>
> 因此，如果一个 ABC 的子类需要被实例化，则必须实现每个虚函数，这也意味着 C++ 支持使用 ABC 声明接口。如果没有在派生类中重载纯虚函数，就尝试实例化该类的对象，会导致编译错误。
>
> 可用于实例化对象的类被称为**具体类**。

抽象类作为基类时必须实现其包含的所有方法，否则继承该抽象类的类仍旧是抽象类，只能继续作为基类使用。下面的代码只实现抽象类中的一个纯虚函数：

```c++
// Person.h
class Person
{
public:
  // 纯虚函数
  virtual void eat() = 0;
  virtual void move() = 0;
};

// main.cpp
class Worker: public Person
{
public:
  Worker() {};
  ~Worker() {};
  // 需要重新声明
  void eat();
  void move();
};

void Worker::eat()
{
  cout << "I can eat.\n";
}

int main()
{
  Worker *worker = new Worker();
  worker->eat();
  delete worker;
  return 0;
}
```

编译器中提示出现如下错误：

`错误(活动)	E0322	不允许使用抽象类类型 "Worker" 的对象`

正确编码如下：

```c++
// Person.h
class Person
{
public:
  // 纯虚函数
  virtual void eat() = 0;
  virtual void move() = 0;
};

// main.cpp
class Worker: public Person
{
public:
  Worker() {};
  ~Worker() {};
  // 需要重新声明
  void eat();
  void move();
};

void Worker::eat()
{
  cout << "I can eat.\n";
}

void Worker::move()
{
  cout << "I can move.\n";
}

int main()
{
  Worker *worker = new Worker();
  worker->eat();
  worker->move();
  delete worker;
  return 0;
}
```

输出：

```
I can eat.
I can move.
```

