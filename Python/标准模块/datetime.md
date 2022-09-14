---
tags: datetime 日期和时间
---

标准模块 datetime，提供了用于处理日期和时间的工具。

# datetime.datetime
datetime.datetime(year, month, day, hour, minute, second, microsecond, tzinfo)
构造一个 datetime 对象。年月日必选，其它可选。
```Python
>>> datetime(2020, 10, 1)
datetime.datetime(2020, 10, 1, 0, 0)
>>> datetime(2022, 1, 13, 20, 14, 50)
datetime.datetime(2022, 1, 13, 20, 14, 50)
```

datetime.datetime.now()
返回一个 datetime 对象，即当前时间。
```Python
from datetime import datetime

>>> datetime.now()
datetime.datetime(2022, 1, 13, 20, 11, 41, 323497)
>>> dt = datetime.now()
>>> dt.year, dt.month, dt.day, dt.hour, dt.minute, dt.second, dt.microsecond
(2022, 1, 13, 20, 11, 48, 928746)
```

datetime 对象可以直接用 str() 或 print() 转换或显示为字符串形式，其日期格式与当前操作系统一致。
```Python
>>> dt = datetime(2022, 1, 13, 20, 14, 50)
>>> str(dt)
'2022-01-13 20:14:50'
>>> print(dt)
2022-01-13 20:14:50
>>> datetime.now().strftime("%Y-%m-%d")
'2022-09-13'
```

datetime 对象的方法 strftime()、strptime()
dt_o.strftime(format): 将该时间对象转换为参数指定格式的字符串。
dt_o.strptime(string, format): 将时间字符串转为 datetime 对象。
```Python
>>> dt = datetime(2022, 1, 13, 20, 14, 50)
>>> dt.strftime('%Y/%m/%d %H:%M:%S')
'2022/01/13 20:14:50'
>>> dt.strftime('%Y年%m月%d日 %H时%M分%S秒')
'2022年01月13日 20时14分50秒'
>>> dt.strftime('%Y年%m月%d日')
'2022年01月13日'

>>> dt.strptime('2022/01/13 20:14:50', '%Y/%m/%d %H:%M:%S')
datetime.datetime(2022, 1, 13, 20, 14, 50)
>>> dt.strptime('2022年01月13日 20时14分50秒', '%Y年%m月%d日 %H时%M分%S秒')
datetime.datetime(2022, 1, 13, 20, 14, 50)
```

如果日期格式中的中文（年月日）报错，可设置：[[locale#^fc29cc\|locale 设置本地环境为中文]]

# datetime.timedelta
timedelta，时间差，即两个日期时间相隔多少天零多少秒。

两个 datetime 对象相减的结果，就是一个 timedelta 对象，该对象有两个属性，days 代表相隔了多少天，seconds 代表除了days 外的秒数。
```Python
>>> from datetime import datetime, timedelta
>>> 
>>> dt1 = datetime(1654, 5, 4, 3, 0)
>>> dt2 = datetime(1722, 12, 20, 2, 54)
>>> k = dt2 - dt1
>>> td
datetime.timedelta(days=25065, seconds=86040)
>>> k.days, k.seconds
(25065, 86040)
```

timedelta(days=value, seconds=vlaue)
构造一个 timedelta 时间差对象。可用于计算间隔指定时间（天/周）的日期。
缺点：只能按天数零秒数来指定时间差，不能按其它如年或月，考虑不到闰年。
```Python
>>> dt = datetime.now()
>>> dt
datetime.datetime(2022, 1, 13, 21, 5, 14, 771775)
>>> k = timedelta(days=365, seconds=0)
>>> dt + k
datetime.datetime(2023, 1, 13, 21, 5, 14, 771775)

# 不能考虑闰年，导致500年后的日期不一致。
>>> k = timedelta(days=365*500, seconds=0)
>>> dt + k
datetime.datetime(2521, 9, 14, 21, 5, 14, 771775)
>>>
>>> from datetime import date, timedelta
>>> date.today()
datetime.date(2022, 9, 13)
>>> date.today() + timedelta(days = 3)
datetime.date(2022, 9, 16)
>>> date.today() + timedelta(days = +3)
datetime.date(2022, 9, 16)
>>> date.today() + timedelta(days = -3)
datetime.date(2022, 9, 10)
>>> date.today() - timedelta(days = 3)
datetime.date(2022, 9, 10)
>>> str(date.today() + timedelta(days = 3))
'2022-09-16'
>>> (date.today() + timedelta(days = 3)).strftime("%Y-%m-%d")
'2022-09-16'
>>> 
>>> def date_n(n):
    	return (date.today() + timedelta(days = int(n))).strftime("%Y-%m-%d")

>>> date_n(3)
'2022-09-16'
>>> date_n(-3)
'2022-09-10'
>>> date_n(0)
'2022-09-13'
```

若要按年或月指定时间差，可参考 [[dateutil#时间差：relativedelta]]
