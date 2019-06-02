---
title: Python 容器
date: 2019-05-17 20:46:04
categorises:
tags:
- python
---

## Python 容器

python 内建的容器数据结构有列表，元组，集合和字典。这四个容器数据结构可以说是 python 编程过程中必不可少的数据类型，合理的选择使用这些数据结构对提升程序性能有很大的帮助。

### 列表

关于列表需要知道的特性有：

- 列表是可变的数据结构，可以向列表里插入和删除元素。
- 列表中可以放任意类型的对象，例如 `[1, 'a', [3.14, 0.5]]`。
- 列表的 `append()` 和无参数的 `pop()` 方法的时间复杂度是 $O(1)$。
- 列表的 `insert()` 操作和带参数的 `pop(index)` 方法的时间复杂度是 $O(n)$。
- `list.sort(), list.reverse()` 是原地排序和翻转的方法；`sorted(), reversed()` 不改变原有参数，而是将排序和翻转后的结果以新对象返回。
- 列表比元组占用更多的内存，性能也差于元组。
- 列表可以当做一个栈结构使用。

```python
>>> dis.dis(lambda : list())
  1           0 LOAD_GLOBAL              0 (list)
              3 CALL_FUNCTION            0 (0 positional, 0 keyword pair)
              6 RETURN_VALUE

>>> dis.dis(lambda : [])
  1           0 BUILD_LIST               0
              3 RETURN_VALUE

>>> %timeit list()
94.3 ns ± 4.23 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

>>> %timeit []
22.2 ns ± 2.37 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

### 元组

关于元组需要知道的特性有：

- 元组是不可变的数据类型，修改元组会引发一个 `TypeError`。
- 元组中也可以存储任意类型的对象，但是如果元组中存储了可变的对象，那么它会变得 `unhashable`，例如 `t = (1, 'a', [3])`。
- 元组的成员函数只有 `count(), index()`。
- 元组可以作为多返回值，也可以用来批量给变量赋值。
- 对元组使用 `sorted(), reversed()` 将会返回一个新列表。
  
```python
>>> dis.dis(lambda : tuple())
  1           0 LOAD_GLOBAL              0 (tuple)
              3 CALL_FUNCTION            0 (0 positional, 0 keyword pair)
              6 RETURN_VALUE

>>> dis.dis(lambda : ())
  1           0 BUILD_TUPLE              0
              3 RETURN_VALUE

>>> %timeit tuple()
70.1 ns ± 6.75 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

>>> %timeit ()
12.9 ns ± 0.0309 ns per loop (mean ± std. dev. of 7 runs, 100000000 loops each)
```

### 集合

关于集合需要知道的特性有：

- 集合可以用来去重。
- 集合的内部实现是散列表，查找元素是否在集合中的平均时间复杂度是 $O(1)$。
- 集合中的对象必须是 `hashable` 的。
- 集合不支持索引操作，但可以用 `in, not in` 判断元素是否在集合内。

### 字典

关于字典需要知道的特性有：

- 字典的键不重复，对同一个键多次进行复制操作只会更新键对应的值。
- 字典也是由散列表实现的，其查找的平均时间复杂度也是 $O(1)$。
- 可以使用 `sorted()` 方法对字典进行排序，但结果会以元组的形式返回。

```python
>>> dis.dis(lambda : dict())
  1           0 LOAD_GLOBAL              0 (dict)
              3 CALL_FUNCTION            0 (0 positional, 0 keyword pair)
              6 RETURN_VALUE

>>> dis.dis(lambda : {})
  1           0 BUILD_MAP                0
              3 RETURN_VALUE

>>> %timeit dict()
133 ns ± 2.95 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

>>> %timeit {}
74.6 ns ± 3.07 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

#### 插入操作

1. index = hash(key) & mask  # mask = PyDicMinSize - 1
2. if slot[index] is empty: slot[index] -> value
3. else if key is old key: slot[index] -> new value
4. else find a new index: slot[new index] -> new value

#### 查找操作

1. index = hash(key) & mask  # mask = PyDicMinSize - 1
2. if slot[index] is empty: raise KeyError
3. if slot[index] is not empty: return slot[index] if slot[index].key == key else find next slot
4. find all slot but not find # can this happen???

#### 删除操作

删除时并不进行删除操作，只是把该位置赋一个特殊的值，得到需要优化字典结构的时候（字典剩余空间小于 1/3），才执行真的删除操作，并在扩容后重新调整结构。
