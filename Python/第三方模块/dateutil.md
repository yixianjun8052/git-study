---
tags: dateutil 时间差
---

dateutil，第三方模块，扩展增强了标准模块 datetime 的功能。

```
pip install python-dateutil
```

# 时间差：relativedelta()
可按年、月、日、天、周、时、分、秒等指定时间差。
```Python
>>> from datetime import datetime
>>> from dateutil.relativedelta import relativedelta
>>> 
>>> dt = datetime.now()
>>> dt
datetime.datetime(2022, 1, 13, 21, 5, 14, 771775)
>>> 
>>> k = relativedelta(years=500)
>>> dt + k
datetime.datetime(2522, 1, 13, 21, 5, 14, 771775)
>>> k = relativedelta(months=3)
>>> dt + k
datetime.datetime(2022, 4, 13, 21, 5, 14, 771775)
```