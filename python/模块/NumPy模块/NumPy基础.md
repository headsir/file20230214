<font size=8>**NumPy基础**</font>

# 一、NumPy入门

## 1.1、NumPy数组

NumPy数组是保存==同构[^注1]数据==的N维数组。数组起始位置左上角。

创建数组：

```python
# coding=UTF-8
# Filename: XXXXX.py
"""
创建数组
"""
# 首先导入NumPy
import numpy as np

# 使用列表构造一个一维数组
array1 = np.array([10, 100, 1000.])
# 使用嵌套列表构造一个二维数组
array2 = np.array([[1., 2., 3.],
                   [4., 5., 6.]])
print(array1.dtype)  # float64,数组数据类型，数组dtype属性
print(type(array2))  # <class 'numpy.ndarray'> ，对象array2类型
```

## 1.2、向量化和广播

对一个标量[^注2]和NumPy数组求和，













---

[^注1]: 同构：所有数据必须是相同类型
[^注2]: 标量：某种Python基本数据类型，是为了将其和列表及字典一类的多元素数据结构以及一维和二维的NumPy数组区分开来。