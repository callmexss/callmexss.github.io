---
layout: port
title: Python 测试
date: 2019-06-02 09:15:58
tags:
- python
- unittest
- doctest
---

# Python 测试框架

介绍 Python 的测试工具 unittest 和 doctest 的基本用法。

<!-- more -->

## unittest

### 基础用法

先来看一个最简单的例子：

```python
import unittest

def add(a, b):
    return a + b

class TestAdd(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(1, 2), 3)
        self.assertEqual(add(-1, 2), 1)


if __name__ == "__main__":
    unittest.main()
```

在上面的例子中通过继承 `unittest.TestCase` 来编写测试类，在测试类中可以使用 `unittest` 提供的若干方法编写测试用例，最后使用 `unittest.main()` 运行单元测试。此外也可以使用 `python -m unittest test.py` 的方式运行测试用例。

### 初始化与清理现场

在一些测试中，需要提前配置一些信息，例如创建某个特性的被测试对象，这个时候可以使用 `unittest` 提供的 `setUp` 函数：

```python
import unittest

class WidgetTestCase(unittest.TestCase):
    def setUp(self):
        self.widget = Widget('The widget')

    def test_default_widget_size(self):
        self.assertEqual(self.widget.size(), (50,50),
                         'incorrect default size')

    def tearDown(self):
        self.widget.dispose()
```

上面一个来自 `Python` 官方文档的样例代码，其中 `setUp` 用于在测试开始前做一些准备工作，`tearDown` 是测试结束后的清理工作，例如测试失败时程序就会终止运行，这个时候可以通过添加 `tearDown` 方法释放系统资源。

### 测试标准输出

有些时候希望测试标准输出是否符合某种期望，可以使用上下文管理器（`context manager`）捕获标准输出。这种方法的本质也是重定向标准输出，使用上下文管理器的好处是不需要在测试结束后还原标准输出，可以避免使用 `try...finally` 块。

```python
import sys
from contextlib import contextmanager
from io import StringIO

@contextmanager
def captured_output():
    new_stdout, new_stderr = StringIO(), StringIO()
    old_stdout, old_stderr = sys.stdout, sys.stderr
    try:
        sys.stdout, sys.stderr = new_stdout, new_std_err
        yield sys.stdout, sys.stderr
    finally:
        sys.stdout, sys.stderr = old_stdout, old_stderr
```

使用方法：

```python
with captured_output() as (out, err):
    foo()
# This can go inside or outside the `with` block
output = out.getvalue().strip()
self.assertEqual(output, 'hello world!')
```

## doctest


## 在 notebook 中使用 unittest 和 doctest

可以在 notebook 中直接使用Python标准测试工具，如doctest或unittest。

### Doctest

在 notebook 的一个方格中定义如下函数和 docstring:

```python
def add(a, b):
    '''
    This is a test:
    >>> add(2, 2)
    5
    '''
    return a + b
```

然后在一个单元格中引入 doctest 库并运行 docstring 中的测试用例：

```python
import doctest
doctest.testmod(verbose=True)
```

得到输出如下：

```text
Trying:
    add(2, 2)
Expecting:
    5
**********************************************************************
File "__main__", line 4, in __main__.add
Failed example:
    add(2, 2)
Expected:
    5
Got:
    4
1 items had no tests:
    __main__
**********************************************************************
1 items had failures:
   1 of   1 in __main__.add
1 tests in 2 items.
0 passed and 1 failed.
***Test Failed*** 1 failures.
```

### Unittest

在 notebook 的一个方格中定义如下函数：

```python
def add(a, b):
    return a + b
```
引入 unitetest 库，然后定义如下测试类：

```python
import unittest

class TestNotebook(unittest.TestCase):

    def test_add(self):
        self.assertEqual(add(2, 2), 5)


unittest.main(argv=[''], verbosity=2, exit=False)
```

输出：

```output
test_add (__main__.TestNotebook) ... FAIL

======================================================================
FAIL: test_add (__main__.TestNotebook)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "<ipython-input-15-4409ad9ffaea>", line 6, in test_add
    self.assertEqual(add(2, 2), 5)
AssertionError: 4 != 5

----------------------------------------------------------------------
Ran 1 test in 0.001s

FAILED (failures=1)
```

### 调式失败测试用例

在调试失败的测试时，在某个时候停止测试用例的执行并运行调试器通常是有用的。为此，请在希望停止执行的行之前插入 `import pdb; pdb.set_trace()`:

```python
def add(a, b):
    '''
    This is the test:
    >>> add(2, 2)
    5
    '''
    import pdb; pdb.set_trace()
    return a + b
```

对于本例，下一次运行 doctest 时，执行将在 return 语句之前停止，Python 调试器(pdb)将启动。您将直接在笔记本中得到一个 pdb 提示符，它将允许您检查 a 和 b 的值，使用 step 和 next 命令等。

## 参考

[unittest — Unit testing framework](https://docs.python.org/3/library/unittest.html)
[How to assert output with nosetest/unittest in python?](https://stackoverflow.com/a/17981937)
[Unit tests for functions in a Jupyter notebook?](https://stackoverflow.com/a/48405555)