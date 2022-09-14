---
tags: itertools 二维列表扁平化
---

itertools，可迭代对象的工具库，内置模块。

itertools.chain(\*iterable): 链条，将可迭代对象参数串成一串，返回的是 Chain 类型的可迭代对象，可用于二维列表转一维，即二维列表扁平化。
```Python
>>> import itertools
>>> 
>>> a1, a2, a3 = [1, 2, 3], [4, 5, 6], [7, 8, 9]
>>> list(itertools.chain(a1, a2, a3))
[1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> type(itertools.chain(a1, a2, a3))
<class 'itertools.chain'>
>>> 
>>> a = [a1, a2, a3]
>>> list(itertools.chain(a))
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
# 解包思想: *a 相当于 a[0] a[1] a[2]
>>> list(itertools.chain(*a))
[1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> 
>>> c = [[4, 2], ['a', 'b'], [3.14, 'lyf', 9]]
>>> list(chain(*c))
[4, 2, 'a', 'b', 3.14, 'lyf', 9]
>>> 
>>> [e for i in a for e in itertools.chain(i)]
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```