---
tags: 偏函数
---

functools，内置函数工具模块。

functools.partial(func, \*args, \*\*kws)
偏函数：如果一个函数有多个参数，如 round(x, n)，那么使用偏函数可以锁定其中某个参数的数值，比如固定为 round(x, 2)。
```Python
>>> pi = 3.14159
>>> round(pi)
3
>>> round(pi, 2)
3.14
>>> import functools
>>> 
>>> help(round)
Help on built-in function round in module builtins:

round(number, ndigits=None)
    Round a number to a given precision in decimal digits.
    
    The return value is an integer if ndigits is omitted or None.  Otherwise
    the return value has the same type as the number.  ndigits may be negative.

>>> 
>>> round2 = functools.partial(round, ndigits=2)
>>> round2(pi)
3.14
>>> round2(1.111111)
1.11
>>> 
>>> import math
>>> 
>>> mylist = [1, 2, 5, 7, 11, 13, 15]
>>> list(map(functools.partial(round, ndigits=2), map(math.sin, mylist)))
[0.84, 0.91, -0.96, 0.66, -1.0, 0.42, 0.65]
```

reduce(function, sequence\[, initial\])
卷地毯
注意：function(x, y)参数函数，必须有两个参数，第一个参数保存的是上一次 reduce 返回的值。
如果不指定初始值 initial，则先从 sequence 中拿前2个元素，传递给 function 处理，处理后的结果交给第一个参数函数 function 的第一个参数 x 保存，再从 sequence 中按顺序拿取下一个元素，交给参数函数 function 的第二个参数 y 参与计算，处理后的结果再交给 x，再如此循环，直到处理完所有元素；
如果指定初始值参数 initial，则先把该初始值 initial 交给 x 保存，再从 sequence 中拿取第1个元素交给 y 保存，参与计算后将结果保存给 x，再按顺序拿取下一个元素，如此循环，直到处理完所有元素。
```Python
>>> from functools import reduce
>>> 
>>> reduce(lambda x, y: x + y, [1, 2, 3, 4, 5])
15
>>> reduce(lambda x, y: x + y, [1, 2, 3, 4, 5], 0)
15
>>> reduce(lambda x, y: x + y*y, [1, 2, 3, 4, 5])
55
>>> reduce(lambda x, y: x + y*y, [1, 2, 3, 4, 5], 0)
55
>>> reduce(lambda x, y: x + y*y, [2, 3, 4, 5], 0)
54
>>> c =  ['关羽', '张飞', '赵云', '马超', '黄忠']
>>> reduce(lambda r, e: r + e[0], c, '')
'关张赵马黄'
>>> ''.join([e[0][0] for e in c])
'关张赵马黄'
```