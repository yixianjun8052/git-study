---
tags: numpy 数值运算 矩阵运算 ndarray
---

NumPy， Numeric Python，数值运算的第三方模块。通常应用于科学计算。
```
pip install numpy

a = numpy.array(o)
a.reshape(行，列)
a.T
a.tolist()
```

| array | 说明 |
|---- | ---- |
| a = numpy.array(o) | 将对象转为numpy数组 |
| a.tolist() | 将数组转为列表 |
| a.reshape(行数, 列数) | 根据原数组a，生成一个指定行列数的新二维数组 |
| a.T | 数组矩阵转置(行变列，列变行) |

numpy.array(object)
numpy.ndarray(o)
N-dimensional array，多维数组，转数组后，可进行线性代数的各种运算。
a.tolist()：将多维数组 ndarray 转成列表。
```Python
>>> import numpy as np
>>> 
>>> a = [i for i in range(10)]
>>> a_array = np.array(a)
>>> a_array
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> type(a_array)
<class 'numpy.ndarray'>
>>> 
>>> a_array * 5
array([ 0,  5, 10, 15, 20, 25, 30, 35, 40, 45])
>>> a_array ** 2
array([ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81], dtype=int32)
>>> b = [i for i in range(10, 0, -1)]
>>> b_array = np.array(b)
>>> a_array + b_array
array([10, 10, 10, 10, 10, 10, 10, 10, 10, 10])
>>> a_array - b_array
array([-10,  -8,  -6,  -4,  -2,   0,   2,   4,   6,   8])
>>> (a_array - b_array).tolist()
[-10, -8, -6, -4, -2, 0, 2, 4, 6, 8]
```

| 运算 | 函数 | 说明 |
| --- | --- | --- |
| 数学运算 | +、-、\*、/、\*\*、log | 加减乘除幂指数 |
| 几何运算 | sin()、cos()、tan() | 正弦、余弦、正切 |
| 统计运算 | sum(a)、max(a)、min(a) | 总和、最值 |
|| median(a) | 中位数 |
|| mean(a)、average(a) | 平均值、加权平均值 |
|| std(a)、var(a)、ptp(a) | 标准差、方差、极差 |
|| argmax(a)、argmin(a) | 返回最值元素所在的下标位置 |
|| percentile(a,q) | 百分位数 |
|| corrcoef(a) | 计算相关系数 |

二维数组使用上述表格中运算函数时，要指定行列轴。
极差：最大值与最小值之差。

一维数组变二维
a.reshape(行数，列数)：根据原数组a，生成一个指定行列数的新二维数组，也可用于二维数组变换行列。
```Python
>>> import random
>>> a = np.array([random.randint(0, 9) for _ in range(12)])
>>> a
array([0, 2, 5, 6, 0, 5, 4, 3, 2, 5, 5, 6])
>>> a.reshape(4, 3)
array([[0, 2, 5],
       [6, 0, 5],
       [4, 3, 2],
       [5, 5, 6]])
>>> a.reshape(4, 3).reshape(2, 6)
array([[0, 2, 5, 6, 0, 5],
       [4, 3, 2, 5, 5, 6]])
>>> a.reshape(4, 3).reshape(2, 6).reshape(1, 12)
array([[0, 2, 5, 6, 0, 5, 4, 3, 2, 5, 5, 6]])
```

矩阵转置
a.T
t, transpose
```Python
>>> b
array([[0, 2, 5],
       [6, 0, 5],
       [4, 3, 2],
       [5, 5, 6]])
>>> b.T
array([[0, 6, 4, 5],
       [2, 0, 3, 5],
       [5, 5, 2, 6]])
```