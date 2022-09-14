---
tags: decimal Decimal 精算
---

标准模块 decimal，内置多种专用于高精度十进制计算的工具。

# Decimal(value='0')
作用：将数字字符串转换为一个“真正的”十进制数字。
参数必须是数字字符串，如'2.37'。如果直接使用数字做参数，相当于先把该数字转换为二进制，再将其近似值交给Decimal()创建十进制数字。

Decimal()创建的数字之间进行数学运算时，使用人类的十进制计算规则（符号0-9、逢十进一）进行计算。该类数字是无法与普通数字直接运算的，必须先将其转换为普通数字，可以使用 int()、float()。

```Python
>>> from decimal import Decimal
>>> Decimal('0.1') + Decimal('0.2')
Decimal('0.3')
>>> print(Decimal('0.1') + Decimal('0.2'))
0.3
>>> 0.3 == Decimal('0.3')
False
>>> 0.3 == float(Decimal('0.3'))
True
```