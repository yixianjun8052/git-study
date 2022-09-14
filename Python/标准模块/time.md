---
tags: time 时间戳 timestamp
---

标准模块 time 提供了与时间的访问和转换相关的函数。

epoch，计算机的时间开始点，其值取决于平台。对于Unix， epoch 是1970年1月1日00:00:00（UTC）。要找出给定平台上的 epoch ，请查看 time.gmtime(0) 。

UTC，协调世界时（Coordinated Universal Time），也被称为格林威治标准时间（GMT）。

time.ctime()：获得当前时间，格式为 'Wed Aug 10 15:01:05 2022'。
```PYTHON
>>> from time import ctime
>>> ctime()
'Wed Aug 10 15:01:05 2022'
```

time.time()
返回当前时间距离 epoch 的秒数，浮点数时间。可用于计算代码的运行时间。时间戳 timestamp。只能计算 epoch 之后的时间，无法表示 epoch 之前的时间。 ^57812c
```Python
import time

>>> time.time()
1642072625.235686

t1 = time.time()
pass
t2 = time.time()
1000*(t2-t1)
```

time.gmtime(\[secs\])
将自 epoch 开始的秒数表示的时间转换为 UTC 的 struct_time。默认返回time()。
```Python
>>> time.gmtime(0)
time.struct_time(tm_year=1970, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=1, tm_isdst=0)
>>> time.gmtime()
time.struct_time(tm_year=2022, tm_mon=1, tm_mday=13, tm_hour=11, tm_min=9, tm_sec=19, tm_wday=3, tm_yday=13, tm_isdst=0)
```

time.localtime(\[secs\])
将自 epoch 开始的秒数表示的时间转换为当地时间的struct_time。默认返回time()。
```Python
>>> time.localtime()
time.struct_time(tm_year=2022, tm_mon=1, tm_mday=13, tm_hour=18, tm_min=58, tm_sec=50, tm_wday=3, tm_yday=13, tm_isdst=0)
```

time.strftime(format\[, tuple\]) -> string
按 format 格式转换时间，返回一个字符串。
```Python
>>> time.strftime('%Y-%m-%d %H:%M:%S')
'2022-01-13 18:39:53'
>>> time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())
'2022-01-13 18:40:59'
>>> time.strftime("%Y/%m/%d %H:%M:%S", time.gmtime())
'2022/07/03 01:24:07'
```

time.strptime(string, format) -> struct_time
根据 format 格式解析表示时间的字符串 string，返回一个struct_time。
```Python
>>> timestr = '2022/1/13 18:50:20'
>>> time.strptime(timestr, '%Y/%m/%d %H:%M:%S')
time.struct_time(tm_year=2022, tm_mon=1, tm_mday=13, tm_hour=18, tm_min=50, tm_sec=20, tm_wday=3, tm_yday=13, tm_isdst=-1)
```

time.sleep(secs)：强制等待，休眠。调用该方法的线程将被暂停执行 secs 秒。

perf_counter()：记录当前时间，两次调用所返回的浮点数时间的差值即是计时时间。