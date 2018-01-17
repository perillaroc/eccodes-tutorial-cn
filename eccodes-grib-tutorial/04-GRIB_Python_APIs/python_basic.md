# Python 和 GRIB API

本章节仅是一个开胃菜，向用户展示 Python 接口提供的很小的一部分功能，用户可以自行探索各种功能。

## Python 是什么

## hello world

```py
#!/usr/bin/env python
import sys
# This is a comment
def say_hello(name):
    print(“Hello "+ name + "!" )
if len(sys.argv) > 1 : name = sys.argv[1]
else:
    name = “World"
say_hello(name)
```

加载需要的模块。

使用**缩进**定义不同的程序块。而不是 `;` 或 `{}` 或 `END`。

使用 `def` 定义函数。

动态变量类型。

处理字符串十分方便。

使用 `python` 运行，或定义 shebang 并配置权限后直接运行。

```
$> python example.py
hello World!
$> ./example.py Xavi
hello Xavi!
```

## 列表和字典（List and dict)

列表类似于 c 语言中的数组，但同一个列表中可以保存不同类型的数据。

```
$> python
>>> mylist = ['a','b','c'] >>> print(mylist)
['a', 'b', 'c']
>>> mylist[2:]
['c']
>>> mylist[-1]
'c'
>>> for element in mylist: ... print(element) ...
a
b
c
```

字典相当于 c++ 中的 `std::map`，可以保存不同种类的数据。

```py
>>> mydict = {'key1':1,'key2':2,'key3':3} >>> for key,value in mydict.items():
... print(key + ":" + str(value)) ...
key3:3
key2:2
key1:1
>>> 'key1' in mydict
True
>>> 'key5' in mydict
False
>>> len(mydict)
3
>>> mydict.keys() ['key3', 'key2', 'key1'] >>> mydict.values()
[3, 2, 1]
```

## NumPy

NumPy 是 Python 科学计算的基础包。提供对多维数组的支持。
同时提供大量对数据进行快速操作的函数，性能与 C 和 Fortran 相当。

越来越多基于 Python 的数据和科学库使用 NumPy。

NumPy 的核心是 ndarray 对象，一个 n 维同构数组。

```py
>>> from numpyimport *
>>> a = arange(15).reshape(3, 5)
>>> a
array([[ 0, 1, 2, 3, 4],
[ 5, 6, 7, 8, 9],
[10, 11, 12, 13, 14]])
>>> a.shape
(3, 5)
>>> a.ndim
2
>>> a.size
15
>>> b = array([6, 7, 8])
>>> b
array([6, 7, 8])
>>> a.sum()
105
>>> a.min()
0
>>> a.max()
14
>>> a.mean()
7.0
>>> b*2
array([12, 14, 16])
>>> b-b
array([0, 0, 0])
>>> b*b
array([36, 49, 64])
```

> 了解NumPy提供哪些函数非常困难。

[http://docs.scipy.org/doc/numpy/reference/](http://docs.scipy.org/doc/numpy/reference/)

数据的操作包括：

- 数学和逻辑
- 变形
- 选择
- IO
- 离散傅里叶变换
- 基本线性代数
- 基本统计函数
- 随机模拟
