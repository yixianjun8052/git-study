itertools，内置迭代器工具模块，提供了操作迭代器的函数。

islice(iterable, stop)，返回一个迭代器。
islice(iterable, start, stop\[, step\])，返回一个迭代器。参数分别为迭代对象、迭代开始位、迭代结束位（不包含）。
```PYTHON
>>> import csv
>>> import codecs
>>> from itertools import islice
>>> 
>>> file = r"C:\Users\yi\Desktop\user_info.csv"
>>> data = csv.reader(codecs.open(file, "r", "utf-8"))
>>> 
>>> conts = islice(data, 1, None)
>>> type(conts)
<class 'itertools.islice'>
>>> 
>>> for line in islice(data, 1, None): line

['byhy', '88888888', '管理员']
['None', '88888888', '请输入用户名']
['', '88888888', '请输入用户名']
[' ', '88888888', '登录失败 : 用户名或者密码错误']
['  byhy  ', '88888888', '登录失败 : 用户名或者密码错误']
['byh', '88888888', '登录失败 : 用户名或者密码错误']
['byhyy', '88888888', '登录失败 : 用户名或者密码错误']
['abcd', '88888888', '登录失败 : 用户名或者密码错误']
['byhy', 'None', '请输入密码']
['byhy', '', '请输入密码']
['byhy', ' ', '登录失败 : 用户名或者密码错误']
['byhy', '  88888888  ', '登录失败 : 用户名或者密码错误']
['byhy', '8888888', '登录失败 : 用户名或者密码错误']
['byhy', '888888888', '登录失败 : 用户名或者密码错误']
['byhy', '12345678', '登录失败 : 用户名或者密码错误']
>>> 
>>> data = csv.reader(codecs.open(r"C:\Users\yi\Desktop\user_info.csv", "r", "utf-8"))
>>> for line in islice(data, 1, 8): line

['byhy', '88888888', '管理员']
['None', '88888888', '请输入用户名']
['', '88888888', '请输入用户名']
[' ', '88888888', '登录失败 : 用户名或者密码错误']
['  byhy  ', '88888888', '登录失败 : 用户名或者密码错误']
['byh', '88888888', '登录失败 : 用户名或者密码错误']
['byhyy', '88888888', '登录失败 : 用户名或者密码错误']
>>> 
>>> data = csv.reader(codecs.open(r"C:\Users\yi\Desktop\user_info.csv", "r", "utf-8"))
>>> for line in islice(data, 1, None, 2): line

['byhy', '88888888', '管理员']
['', '88888888', '请输入用户名']
['  byhy  ', '88888888', '登录失败 : 用户名或者密码错误']
['byhyy', '88888888', '登录失败 : 用户名或者密码错误']
['byhy', 'None', '请输入密码']
['byhy', ' ', '登录失败 : 用户名或者密码错误']
['byhy', '8888888', '登录失败 : 用户名或者密码错误']
['byhy', '12345678', '登录失败 : 用户名或者密码错误']
```