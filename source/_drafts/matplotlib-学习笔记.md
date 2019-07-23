---
title: matplotlib 学习笔记
date: 2019-06-04 14:53:45
categorises:
tags:
- python
- matplotlib
- visualization
---

## Matplotlib 简介

## 两种使用模式（MATLAB接口和面向对象）

## Cheat Sheet

### 限定横纵坐标轴

### 浮点数定步长迭代

```python
>>> np.linspace(0,1,11)
array([ 0. ,  0.1,  0.2,  0.3,  0.4,  0.5,  0.6,  0.7,  0.8,  0.9,  1. ])
>>> np.linspace(0,1,10,endpoint=False)
array([ 0. ,  0.1,  0.2,  0.3,  0.4,  0.5,  0.6,  0.7,  0.8,  0.9])
```

```python
>>> import numpy as np
>>> np.arange(0.0, 1.0, 0.1)
array([ 0. ,  0.1,  0.2,  0.3,  0.4,  0.5,  0.6,  0.7,  0.8,  0.9])
```

```python
>>> numpy.arange(1, 1.3, 0.1)
array([1. , 1.1, 1.2, 1.3])
```

## 引用

[How to use a decimal range() step value?](https://stackoverflow.com/questions/477486/how-to-use-a-decimal-range-step-value)
