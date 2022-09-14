---
tags: excel
---

pandas, Panel Date 面板数据分析。用于数据统计的第三方模块。基于numpy模块。
```
pip install pandas

pip install openpyxl
```

pandas.read_excel(fpath, sheetname)

一维数据类型 series
多维数据类型 dataframe

dataframe.iloc\[首行:尾行, 首列:尾列\]
dataframe.to_excel(fpath, sheetname): 新创建excel写入，如果文件存在则覆盖写。

```Python
>>> import pandas
>>> 
>>> b = pandas.read_excel(r"C:\Users\yi\Desktop\degree.xlsx", '计件工资')
>>> type(b)
<class 'pandas.core.frame.DataFrame'>
>>> b.iloc[2:, 2:]
  Unnamed: 2 Unnamed: 3 Unnamed: 4
2       6800       6500       5400
3       5600       6000       5700
4       6600       5670       6100
5       5100       6540       5500
>>> b.iloc[2:, 2:].values
array([[6800, 6500, 5400],
       [5600, 6000, 5700],
       [6600, 5670, 6100],
       [5100, 6540, 5500]], dtype=object)
>>> 
>>> c = pandas.read_excel(r"C:\Users\yi\Desktop\degree.xlsx", '出勤奖励')
>>> c.iloc[2:,3]
2     360
3     370
4     355
5     340
6     350
7     355
8     400
9     355
10    365
11    380
12    375
13    390
Name: Unnamed: 3, dtype: object
>>> type(c.iloc[2:,3])
<class 'pandas.core.series.Series'>
>>> c.iloc[2:,3].values
array([360, 370, 355, 340, 350, 355, 400, 355, 365, 380, 375, 390],
      dtype=object)
>>> c.iloc[2:,3].values.reshape(4, 3)
array([[360, 370, 355],
       [340, 350, 355],
       [400, 355, 365],
       [380, 375, 390]], dtype=object)
>>> 
>>> (b.iloc[2:, 2:].values + c.iloc[2:,3].values.reshape(4, 3)).T
array([[7160, 5940, 7000, 5480],
       [6870, 6350, 6025, 6915],
       [5755, 6055, 6465, 5890]], dtype=object)
```

统计函数
count()、sum()、max()、min()、mean()、median()、std()等等。