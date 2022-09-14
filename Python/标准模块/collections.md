---
tags: Counter计数器
---

collections，内置模块。

collections.Counter(iterable)：计数器，计算可迭代对象元素的频次统计，返回的 Counter 类是一个特殊的字典类对象，可按照字典进行操作，也可进行加法、减法、交集、并集等运算，减法只保留值为正数的键值对。
```Python
>>> from collections import Counter
>>> 
>>> a = ['A', 'B', 'C', 'D', 'B', 'A', 'D', 'E', 'F', 'C', 'A', 'B', 'G']
>>> b = ['A', 'C', 'E', 'H', 'X', 'Y', 'Z', 'A', 'C', 'B']
>>> a1, b1 = Counter(a), Counter(b)
>>> a1
Counter({'A': 3, 'B': 3, 'C': 2, 'D': 2, 'E': 1, 'F': 1, 'G': 1})
>>> b1
Counter({'A': 2, 'C': 2, 'E': 1, 'H': 1, 'X': 1, 'Y': 1, 'Z': 1, 'B': 1})
>>> a1 + b1
Counter({'A': 5, 'B': 4, 'C': 4, 'D': 2, 'E': 2, 'F': 1, 'G': 1, 'H': 1, 'X': 1, 'Y': 1, 'Z': 1})
>>> a1 - b1
Counter({'B': 2, 'D': 2, 'A': 1, 'F': 1, 'G': 1})
>>> b1 - a1
Counter({'H': 1, 'X': 1, 'Y': 1, 'Z': 1})
>>> a1 & b1
Counter({'A': 2, 'C': 2, 'B': 1, 'E': 1})
>>> a1 | b1
Counter({'A': 3, 'B': 3, 'C': 2, 'D': 2, 'E': 1, 'F': 1, 'G': 1, 'H': 1, 'X': 1, 'Y': 1, 'Z': 1})
>>> 
>>> type(Counter(a))
<class 'collections.Counter'>
>>> a1['A']
3
```

collections.OrderedDict()：会根据放入元素的先后顺序进行排序，所以输出的值是排好序的，用于生成一个有序字典。