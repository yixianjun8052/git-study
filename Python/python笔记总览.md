# 语言基础
官网：https://www.python.org。

Python 语言是由 Guido van Rossum 于 1989 年开发的，于 1991 年发行第一个公开发行版。因为早期的 Python 版本在基础设计方面存在着一些不足之处，所以 2008 年 Guido van Rossum 重新发布了 Python （当时命名为 Python 3000）。Python 3 在设计的时候很好地解决了之前的遗留问题，但是 Python 3 的最大的问题是不完全向后兼容，当时向后兼容的版本是 Python 2.6。

Guido van Rossum 宣布对 Python 2.7 的技术支持时间延长到 2020 年。Python 2.7 是 2.x 系列的最后一个版本。

推荐使用 Python 3.x。

## 编写程序步骤
问题分析：研究需要解决的问题，确定要解决的问题是什么
规格说明：确定程序要做什么，如确定输入、输出和之间的关系
算法设计：规划程序的总体结构，确定程序怎么做。可用伪代码编写算法
实现设计：将设计翻译成编程语言
测试/调试：查找和修复程序中的错误
维护：让程序保持最新，满足不断变化的需求

## 环境与编辑风格
Python 是一门解释型语言，官方标准解释器 CPython 可到[官网](https://www.python.org/) 下载。
安装时请选择 "Add Python to PATH"，否则安装后需要自行设置环境变量。
检验安装配置 cmd：python，出现版本号并进入 Python 命令行即成。

IDE，集成开发环境，有IDLE、PyCharm。

## 语法
[[编码风格]]

[[变量、标识符和关键字]]

[[数据类型|数据类型及其方法、数据类型转换]]

[[流程控制|流程控制：if 判断、for/while 循环、try except 异常处理]]

[[内置函数]]、[[自定义函数]] 

## 代码调试
代码调试步骤：单步执行、设置断点、观察变量。
调试时，如果进入函数内部，可用“Out”跳出。"Over"可跳过，不会弹出源代码。

## 模块和包
模块 module，就是一个 .py 文件，模块的名字就是文件的名字，有标准模块、第三方模块和自定义模块。标准库存放在Python安装目录的 Lib 目录下，第三方模块默认存放在 Lib/site-packages/ 目录下。

模块导入：`[from ...] import ... [as ...]`
导入模块时，会将被导入模块中的可执行的内容全部执行一遍。何时导入何时执行，重复导入不再执行。可以只导入模块、模块中的指定类或函数。

模块导入 | 调用
---- | ----
import 包/模块 | 模块名.函数名/类/全局属性
from 包/模块 import 模块/函数/类/全局属性 | 无需模块名，直接调用
from 包/模块 import \* | 若模块中使用了 \_\_all\_\_ = [] ，则只能使用 \_\_all\_\_ 列表中指定的字符串内容。
as alias | 起别名，一旦起了别名就只能用别名。\* 通配符，不建议使用。

查看模块的帮助文档：help()
```PYTHON
>>> import time
>>> help(time)
```

在测试模块时，为了避免执行开发自写的测试代码，可利用python自带变量 \_\_name\_\_ 来控制：在本模块中执行时值为"\_\_main\_\_"，在被其他模块导入执行时值为被导入模块名称。

即本模块的自测代码应该写在 `if __name__ == '__main__':` 中，表示当模块被直接运行时，下面的代码块将被运行；当模块被其他程序文件调用时，下面的代码块不被运行。

```PYTHON
>>> __name__
'__main__'
>>> import os
>>> os.__name__
'os'
```

\_\_file\_\_：本模块的完整存储路径，如修改内容则该模块只能运行这一次，等于自杀。
```python
# test1.py
import module as aliases
import package.module as aliases
from module import *
from module import class as aliases, func as aliases
from package.module import class as aliases, func as aliases

__all__ = ["country", "add2num"]

country = "中国"

def add2num(a, b):
    return a + b

class Person(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return "%s %d" % (self.name, self.age)

    def eat(self):
        print("会吃饭")


if __name__ == "__main__":
    # 本模块自测代码
    print(__name__)
    print(__file__)
    
    print(country)
    print(add2num(3, 5))
    liuyifei = Person("刘亦菲", 18)
    print(liuyifei)
    liuyifei.eat()


# test2.py
import test1 as t1
t1.__name__
t1.__file__

print(t1.country)
print(t1.add2num(3, 5))

# 下列代码报错，不能调用
liuyifei = Person("刘亦菲", 18)
```

导入模块的搜索原理：先在内置模块里面找，找不到再到 sys.path 里面的路径中搜索。
sys.path里面的路径来源于：1、启动脚本文件所在的目录，2、PYTHONPATH 环境变量里的目录，3、python 解释器的缺省安装目录。
```PYTHON
>>> import sys
>>> print(sys.path)
['', 'D:\\Programs\\Python39\\Lib\\idlelib', 'D:\\workspace\\pyproject\\bysms', 'D:\\Programs\\Python39\\python39.zip', 'D:\\Programs\\Python39\\DLLs', 'D:\\Programs\\Python39\\lib', 'D:\\Programs\\Python39', 'D:\\Programs\\Python39\\lib\\site-packages', 'D:\\Programs\\Python39\\lib\\site-packages\\win32', 'D:\\Programs\\Python39\\lib\\site-packages\\win32\\lib', 'D:\\Programs\\Python39\\lib\\site-packages\\Pythonwin']
```

**跨目录调用模块**

假设文件目录如下，test.py 文件要想调用 calculator.py 文件就比较麻烦了。
>project2/
>├── module/
>│ └──calculator.py
>└── test/
>└── test.py

calculator.py 文件的绝对路径是：
>D:\\git\\book-code\\python_base\\project2\\module

明白了这一点，跨目录调用文件的问题就很好解决了，我们只需将 calculator.py 文件所属的目录添加到系统的 Path 中即可。
```PYTHON
import sys
sys.path.append("D:\\git\\book-code\\python_base\\project2\\module")
from calculator import add
print(add(2, 3))
```
这样，问题就解决了。但是，当代码中出现 D:\\git\\book-code\\python_base\\project2\\module 这样的绝对路径时，项目的可移植性就会变得很差，因为你写的代码需要提交到Git，其他开发人员需要拉取这些代码并执行，你不能要求所有开发人员的项目路径都和你保持一致。所以，我们应该避免在项目中写绝对路径。
进一步优化代码如下：
```PYTHON
import sys
from os.path import dirname, abspath

project_path = dirname(dirname(abspath(__file__)))
sys.path.append(project_path + "\\module")
from calculator import add

print(add(2,3))
```

\_\_file\_\_ 用于获取文件所在的路径，调用 os.path 下面的 abspath(__file__) 可以得到文件的绝对路径：
>D:\\git\\book-code\\python_base\\project2\\test\test.py

dirname()函数用于获取上级目录，所以当两个 dirname()函数嵌套使用时，得到的目录如下：
>D:\\git\\book-code\\python_base\\project2

将该路径与“\\module”目录拼接，可得到 calculator.py 文件的所属目录，添加到 Path 即可。这样做的好处是，只要 project2 项目中的目录结构不变，在任何环境下都可以正常执行。

**包 package**

将有联系的模块组织在一起，即放到同一个文件夹下，并且在这个文件夹创建一个名字为__init__.py 文件，那么这个文件夹就称之为包。相当于一个文件夹，里面存放相关的模块文件，包含 \_\_init\_\_.py、\_\_main\_\_.py和其他模块文件。

包有效的避免了模块名称冲突问题，让应用组织结构更加清晰。

\_\_init\_\_.py，作为包的标志文件，控制着包的导入行为，也可以写代码，在被导入时执行。若该文件内容为空，或该文件中的变量 \_\_all\_\_ = \[\]，那么，导入包的操作仅仅只是导入了包，不会导入包中的模块；若该文件中的变量 \_\_all\_\_ = \[模块1, 模块2\]，那模块1和2可以被导入。\_\_all\_\_ 变量控制着【from 包名 import \*】时导入的模块。

\_\_main\_\_.py：主程序文件。将包内模块文件压缩后（mymath.zip），cmd“python d:/mymath.zip”运行时，运行的是该主程序文件。

Package | \_\_init\_\_.py | 其他模块
---- | ---- | ----
\_\_init\_\_.py<br>module.py<br>module2.py | \_\_all\_\_ = \[“module”, ....\]<br>print("刘亦菲") | import package<br>from package import module<br>from package import \*

## 可迭代对象
迭代，iterate，按某种顺序逐个访问。

可迭代对象，iterable，可以包含多个元素，并且可以用 for in 循环逐个访问内部元素的数据结构，包括 str、set、 list、tuple、dict 等常见容器，range对象，迭代器，以及其他可迭代对象。

| 可迭代对象 | 细分 | 例子 |
| --- | --- | --- |
| 常见容器 | | 列表、元组、字典、集合 |
| range()对象 |||
| 迭代器 | 普通迭代器 | zip() |
|| 生成器 | (生成规则) 或 yield ||
| 其他可迭代对象 || 字符串 |

### 迭代器
迭代器，iterator，可用 next() 遍历的可迭代对象，但只能访问一次，一旦读取过就无法再次读取，相当于阅后即焚。包括普通迭代器和生成器。
普通迭代器，通过编写特殊的类实现。如 zip()。
生成器，Generator，通过圆括号 ()  或 yield 关键字生成。如  (x for x in \[1, 2, 3\])。

迭代器的懒惰策略
普通容器，存入的是数据，占用大量内存，可反复读取，随用随取，无需再计算。
range()和迭代器，存入的是生成规则或计算公式，占用少量的内存，先不计算，只记住规则，直到程序要用到这个数据时才临时耗时从头计算，即延迟计算或懒惰策略。算完即清理，不浪费内存空间，以时间换空间。range()可反复读取，迭代器只能读取一次，要再次读取必须重新定义。

```python
>>> t = list(range(1000000000))
Traceback (most recent call last):
  File "<pyshell#151>", line 1, in <module>
    t = list(range(1000000000))
MemoryError
>>> t1 = range(1000000000)
>>> t1[268]
268
>>> 
>>> c = [x * y for x in range(1, 1000001) for y in range(1, 1001)]
Traceback (most recent call last):
  File "<pyshell#211>", line 1, in <module>
    c = [x * y for x in range(1, 1000001) for y in range(1, 1001)]
  File "<pyshell#211>", line 1, in <listcomp>
    c = [x * y for x in range(1, 1000001) for y in range(1, 1001)]
MemoryError
>>> c = (x * y for x in range(1, 1000001) for y in range(1, 1001))
>>> 
```

元素访问
range()可使用下标索引和len()。
迭代器没有 len()，也不能使用下标索引的方式获取元素，但可使用 next() 从第1个元素开始，逐个找到对应的元素，直到最后全部读完，抛出异常。只能从头向读取一次。要访问第几个元素，可先用循环 for i in range(n): next(iter) 跳过前几个元素，再访问 next(iter)。
```python
>>> for i in (i*i for i in range(1, 11)):
    	i

	
1
4
9
16
25
36
49
64
81
100
>>> x = (i*i for i in range(1, 11))
>>> next(x)
1
>>> next(x)
4
>>> for i in range(5):
	next(x)

	
9
16
25
36
49
>>> next(x)
64
>>> next(x)
81
>>> next(x)
100
>>> next(x)
Traceback (most recent call last):
  File "<pyshell#31>", line 1, in <module>
    next(x)
StopIteration
```

### 解包
解包，就是多重变量赋值。字符串、列表、元组、字典、集合都可以解包。可用于多重变量赋值、变量值交换。
var1, var2, ... = str/list/tuple/dict/dict.values()/dict.items()/set

星号\*的含义：将剩余内容放入列表，赋值给变量。
分配规则：先处理位置明确的变量，然后再将剩余内容作为列表赋值给星号变量。即使只剩一个元素，也会放入列表。
```python
>>> a, b, c = [1, 2, 3, 4]
Traceback (most recent call last):
  File "<pyshell#94>", line 1, in <module>
    a, b, c = [1, 2, 3, 4]
ValueError: too many values to unpack (expected 3)
>>> *a, b, c = [1, 2, 3, 4]
>>> a, b, c
([1, 2], 3, 4)
>>> a
[1, 2]
>>> a, *b, c = [1, 2, 3, 4]
>>> a, b, c
(1, [2, 3], 4)
>>> a, b, *c = [1, 2, 3, 4]
>>> a, b, c
(1, 2, [3, 4])
>>> a, b, *c = [1, 2, 3]
>>> a, b, c
(1, 2, [3])
>>> a, *b, *c = [1, 2, 3, 4]
SyntaxError: multiple starred expressions in assignment
```

可用于计分：去掉最高分、去掉最低分、剩余分数
```python
>>> score = [9.5, 4.6, 8.4, 9.3, 2.9, 5.7, 7.4]
>>> score.sort()
>>> score
[2.9, 4.6, 5.7, 7.4, 8.4, 9.3, 9.5]
>>> s_min, *sc, s_max = score
>>> s_min, s_max, sc
(2.9, 9.5, [4.6, 5.7, 7.4, 8.4, 9.3])
```

### 容器常见问题
1，如果函数参数的默认值使用可变对象，如列表、字典、集合，会导致每次调用时默认值变化。
该默认值在程序运行之初的“编译阶段”就已经被创建，并一直保存在内存中。所以每次调用函数，修改的是同一默认值对象。
```python
>>> def add_END(t=[]):
	t.append('END')
	return t

>>> add_END()
['END']
>>> add_END()
['END', 'END']
>>> add_END()
['END', 'END', 'END']
```

2，浅拷贝与深拷贝区分
等号引用
```python
>>> a = ['A', 'B', 'C']
>>> b = a
>>> b[0] = 'b'
>>> b
['b', 'B', 'C']
>>> a
['b', 'B', 'C']
>>> 
>>> c = a.copy()
>>> c[0] = 'c'
>>> c
['c', 'B', 'C']
>>> a
['b', 'B', 'C']
```

浅拷贝：copy()，如果是多层列表（或字典等其他容器），只会复制第一层的内容，不会复制深层子容器。列表、字典的copy()都是浅拷贝。
```python
>>> a = ['A', ['B', 'C'], 'D']
>>> b = a.copy()
>>> b
['A', ['B', 'C'], 'D']
>>> b[1][0] = 'b'
>>> b
['A', ['b', 'C'], 'D']
>>> a
['A', ['b', 'C'], 'D']
```

深拷贝：全部复制包括深层子容器。实现深拷贝可用内置模块的 copy.deepcopy()或用到递归算法自定义程序，列表、字典都可用。
```python
>>> import copy
>>> 
>>> a = ['A', ['B', 'C'], 'D']
>>> b = copy.deepcopy(a)
>>> b
['A', ['B', 'C'], 'D']
>>> b[1][0] = 'bB'
>>> a
['A', ['B', 'C'], 'D']
>>> b
['A', ['bB', 'C'], 'D']
```

3，不要用浅拷贝列表乘法创建二维列表
二维列表用乘法\*是浅拷贝。推荐使用列表生成式创建二维列表。
```python
>>> a = [0] * 3
>>> a
[0, 0, 0]
>>> a[0] = 999
>>> a
[999, 0, 0]
>>> 
>>> a = [0, 1, 2]
>>> b = [a] * 3
>>> b
[[0, 1, 2], [0, 1, 2], [0, 1, 2]]
>>> b[0][1] = 999
>>> b
[[0, 999, 2], [0, 999, 2], [0, 999, 2]]
>>> id(b[0]);id(b[1]);id(b[2])
1809725862720
1809725862720
1809725862720
>>> id(a)
1809725862720
>>> 
>>> c = [[0, 0, 0] for i in range(2)]
>>> c
[[0, 0, 0], [0, 0, 0]]
>>> id(c[0]);id(c[1])
1809725857088
1809725769792
>>> c[0][0] = 999
>>> c
[[999, 0, 0], [0, 0, 0]]
>>> 
>>> c = [[0]*3 for _ in range(2)]
>>> c
[[0, 0, 0], [0, 0, 0]]
>>> id(c[0]);id(c[1])
1809725676672
1809725770688
```

## 数据处理
[[文件处理|文本文件的打开、处理和关闭]]
### 字符串比较
字符串比较，采用逐位字符进行比较，每个字符之间的比较本质上是ASCII码之间的数值比较。
ASCII码，American Standard Code for Information Interchange 美国信息交换标准代码，是一套人为规定的字符（控制字符、特殊符号、标点符号、运算符号、数字、字母）与数字之间的映射关系表。
查看字符的ASCII码：`ord(c)`
查看数字对应的Unicode字符：`chr(i)`
转义字符标识，反斜线：`\`
```python
>>> 'A' < 'a';ord('A');ord('a')
True
65
97
>>> '3' < '4';ord('3');ord('4')
True
51
52
>>> '237' < '24'
True
>>> '237' > '23'
True
>>> '237' == '237'
True
>>> 'hello' > 'he'
True
>>> 'hello' > 'hi';ord('e');ord('i')
False
101
105
>>> 
>>> s = '白日依山尽' + chr(10) + '黄河入海流' + chr(10) + '欲穷千里目' + chr(10) + '更上一层楼'
>>> s
'白日依山尽\n黄河入海流\n欲穷千里目\n更上一层楼'
>>> print(s)
白日依山尽
黄河入海流
欲穷千里目
更上一层楼
>>> s2 = '白日依山尽\n黄河入海流\n欲穷千里目\n更上一层楼'
>>> print(s2)
白日依山尽
黄河入海流
欲穷千里目
更上一层楼
>>> ord('\n');chr(10)
10
'\n'
>>> ord(' ');chr(32)
32
' '
>>> print('hello' + chr(32) + 'liuyifei')
hello liuyifei
>>> 
>>> print('hello\nlyf');print('hello\lyf')
hello
lyf
hello\lyf
```

例子：将分数等级用数字代替字母。
```python
>>> scores = {'张三': 'A', '李四': 'B', '王五': 'C', '赵六': 'D',}
>>> ord('A');ord('B');ord('C');ord('D')
65
66
67
68
>>> {k:ord(v)-64 for k, v in scores.items()}
{'张三': 1, '李四': 2, '王五': 3, '赵六': 4}
>>> 
>>> {k:ord(scores[k])-64 for k in scores}
{'张三': 1, '李四': 2, '王五': 3, '赵六': 4}
>>> 
>>> {k:ord(v)-(ord('A')-1) for k, v in scores.items()}
{'张三': 1, '李四': 2, '王五': 3, '赵六': 4}
>>> {k:(ord(v)-(ord('A')-1)) for k, v in scores.items()}
{'张三': 1, '李四': 2, '王五': 3, '赵六': 4}
```

### 编解码
文本文件乱码问题八字终极解决方案：打回原形（bytes），重塑金身。
说明：bytes通过decode()转为str，再通过encode()转为新bytes，并写入新文件。

国标码：GB2312、GBK、GB18030。汉字推荐用最新的强制标准 GB18030，可以用判断语句指定。

open()打开文件时，如果不指定 encoding，则按照系统默认的编码方案读取。中文Windows一般默认是GB。推荐指定 encoding 参数。

encoding 参数的值，最好是先获取到文件的编码，再打开时直接指定。具体实现可以参考第三方模块 [[chardet]]。

字符串互转字节流
str.encode(编码名称)：将字符串按指定编码转换成一串字节数据，返回bytes对象。
bytes.decode(编码名称)：解码，将字节流按指定编码转换成字符串并返回。
```python
>>> s = '大家好'
>>> 
>>> b = s.encode('gb18030')
>>> b
b'\xb4\xf3\xbc\xd2\xba\xc3'
>>> len(b)
6
>>> list(b)
[180, 243, 188, 210, 186, 195]
>>> b[0]
180
>>> b.decode('gb18030')
'大家好'
>>> chardet.detect(b)
{'encoding': 'ISO-8859-1', 'confidence': 0.73, 'language': ''}
>>> 
>>> b2 = s.encode('utf-8')
>>> b2
b'\xe5\xa4\xa7\xe5\xae\xb6\xe5\xa5\xbd'
>>> len(b2)
9
>>> list(b2)
[229, 164, 167, 229, 174, 182, 229, 165, 189]
>>> b2[0]
229
>>> b2.decode()
'大家好'
>>> chardet.detect(b2)
{'encoding': 'utf-8', 'confidence': 0.87625, 'language': ''}
```

### 读取文件并写入（未知编码）
步骤：
1，读取文件的字节流：‘rb'
2，识别编码：chardet.detect(bytes)
3，将字节流转换成字符串，bytes.decode()
4，将字符串转换成指定编码的字节流，str.encode()
5，将第4步的字节流写入新文件：’wb'
```python
>>> import chardet
>>> 
>>> file = "C:/Users/yi/Desktop/新建文本文档.txt"
>>> with open(file, 'rb') as f:
    	s = f.read()



>>> e = chardet.detect(s)['encoding']
>>> if e.lower().startswith('bg'):
        e = 'gb18030'
>>> 
>>> s.decode(e)
'陕北民歌\r\n\t丁文军 - 小桃红\r\n\t刘妍 - 脸蛋蛋烫了哥哥的手、为甚不回家\r\n山西民歌\r\n\t圪梁梁\r\n神仙挡不住人想人\r\ntest'
>>> 
>>> newfile = 'C:/Users/yi/Desktop/新建文本文档1.txt'
>>> with open(newfile, 'wb') as f:
        f.write(s.decode(e).encode('utf-8'))

	
151
>>> with open(newfile, 'rb') as f:
        s = f.read()

	
>>> chardet.detect(s)
{'encoding': 'utf-8', 'confidence': 0.99, 'language': ''}
```

### 正则匹配
写正则式参考：[[re]]

## 异常
Python 用异常对象（Exception Object）来表示异常情况。在遇到错误后，异常对象会引发异常。如果异常对象并未被处理或捕捉到，则程序会用回溯（Traceback，一种错误信息）来终止程序。

在程序开发中，有时程序并不按照我们设计的那样工作，它也有“生病”的时候。这时我们就可以通过异常处理机制，有预见性地获得这些“病症”，并开出“药方”。例如，一个普通人，在寒冷的冬天洗冷水澡，那么他很可能会感冒。我们可以在他洗冷水澡之前提前准备好感冒药，假如他真感冒了，就立刻吃药。

异常的抛出机制？
1．如果在运行时发生异常，那么解释器会查找相应的处理语句（称为 handler）。
2．如果在当前函数里没有找到相应的处理语句，那么解释器会将异常传递给上层的调用函数，看看那里能不能处理。
3．如果在最外层函数（全局函数 main()）也没有找到，那么解释器会退出，同时打印 Traceback，以便让用户找到错误产生的原因。

虽然大多数错误会导致异常，但异常不一定代表错误，有时候它们只是一个警告，有时候是一个终止信号，如退出循环等。

既然知道当打开一个不存在的文件时会抛出 FileNotFoundError 异常，那么我们就可以通过 Python 提供的 try except 语句来捕捉并处理这个异常。

在 Python 中，所有的异常类都继承自 Exception。但自 Python 2.5 版本之后，所有的异常类都有了新的基类 BaseException。Exception 同样也继承自 BaseException，所以我们可以使用 BaseException 来接收所有类型的异常。

我们在 BaseException 后面定义了 msg 变量来接收异常信息，并通过 print()将其打印出来。
```PYTHON
>>> try:
	open("abc.txt", 'r')
	print(a)
except BaseException as msg:
	print(msg)

	
[Errno 2] No such file or directory: 'abc.txt'

```

异常更多用法
```PYTHON
try:
    a = "异常测试："
    print(a)
except NameError as msg:
    print(msg)
else:
    print("没有异常时执行!")

try:
    print(a)
except NameError as msg:
    print(msg)
finally:
    print("不管是否出现异常，都会被执行。")
```

**Python 中常见的异常**

异常 | 描述
---- | ----
BaseException | 新的所有异常类的基类
Exception | 所有异常类的基类，但继承自 BaseException 类
AssertionError | assert 语句失败
FileNotFoundError | 试图打开一个不存在的文件或目录
AttributeError | 试图访问的对象没有属性
OSError | 当系统函数返回一个系统相关的错误（包括 I/O 故障），如“找不到文件”或“磁盘已满”时，引发此异常
NameError | 使用一个还未赋值对象的变量
IndexError | 当一个序列超出范围时引发此异常
SyntaxError | 当解析器遇到一个语法错误时引发此异常
KeyboardInterrupt | 组合键 Ctrl+C 被按下，程序被强行终止
TypeError | 传入的对象类型与要求不符

**抛出异常**
如果你开发的是一个第三方库或框架，那么在程序运行出错时抛出异常会更为专业。
在 Python 中，raise 关键字可用来抛出一个异常信息。下面的例子演示了 raise 的用法。
```PYTHON
>>> def say_hello(name=None):
	if name is None:
		raise NameError('"name" cannot be empty')
	else:
		print("hello, %s" %name)

		
>>> say_hello()
Traceback (most recent call last):
  File "<pyshell#7>", line 1, in <module>
    say_hello()
  File "<pyshell#6>", line 3, in say_hello
    raise NameError('"name" cannot be empty')
NameError: "name" cannot be empty
```

需要注意的是，raise 只能使用 Python 提供的异常类，如果想要 raise 使用自定义异常类，则自定义异常类需要继承 Exception 类。

## 单元测试
assert expression
```PYTHON
>>> assert 1 + 2 == 3
>>> assert 1 + 2 != 3, "加法运算失败！"
Traceback (most recent call last):
  File "<pyshell#12>", line 1, in <module>
    assert 1 + 2 != 3, "加法运算失败！"
AssertionError: 加法运算失败！
```

使用 assert 调试性断言存在三个问题，一是需要自己定义断言失败的提示，二是一旦某个测试函数断言不通过，后面的测试函数将不再执行，三是测试结果无法统计。

推荐使用 [[unittest]] 做单元测试。

# 编程思维
## 自顶向下设计
一种成熟的解决复杂问题的技术称为“自顶向下”。基本思想是从总问题开始，尝试用较小的问题来表达解决方案。然后依次使用相同的技术，攻克每个较小的问题。最终，问题变得如此之小，以至于它们可以轻松得到解决。然后把所有的小块都拼回来，大功告成，你得到了一个程序。

## 自底向上实现
单元测试，从结构图的最底层开始，并在完成每个组件时测试它。

## 原型与螺旋式开发
从程序或程序组件的简单版本开始，然后尝试逐渐添加功能，直到满足完整的规格说明。

初始的朴素版本称为“原型”。先设计、实现并测试一个原型。然后新功能被设计、实现和测试。在开发过程中，我们完成许多小循环，原型逐渐扩展为最终的程序。

面对新的或不熟悉的功能或技术时，螺旋式开发特别有用。

螺旋式开发不是自顶向下设计的替代品。相反，它们是互补的方法。在设计原型时，你仍然会使用自顶向下的技术。

## 面向对象编程
面向过程编程：根据业务逻辑从上到下写代码。将数据与函数按照执行的逻辑顺序组织在一起，数据与函数分开考虑。

面向对象编程： Object Oriented Programming-OOP，把软件系统中相近相似的操作逻辑和操作、应用数据、状态，以类的型式描述出来，以对象实例的形式在软件系统中复用。就是封装和调用。

### 类和对象
定义类：`class 类名:pass`
命名规则：类用大驼峰，方法用下划线。

定义类，意味着在内存中开辟一块地址存储类，类名一是作为类的 \_\_name\_\_ 属性的值，一是作为变量指向这个类的地址。对象对应的类通过 \_\_class\_\_ 指定。

获取类的信息：type(o)、isinstance(o, class)、dir(o)、help()、类.mro() 返回本类及其所有父类

对象是类的实例化。从类到对象，是一个模板复制的过程，每个对象独立不想干。万物皆对象。
```python
>>> class ClassName:
        pass

>>> ob = ClassName()
>>> 
>>> type(ob)
<class '__main__.ClassName'>
>>> 
>>> id(ob)
2142853791264
>>> 
>>> dir(ob)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>> ob.__class__
<class '__main__.ClassName'>
```

####  self 自动参数传递
方法的创建同样使用 def 关键字，与函数的唯一区别是，方法的第一个参数必须声明，一般习惯命名为“self”，但在调用这个方法时并不需要为该参数设置数值。
当调用一个对象的某个方法时，Python 会自动为其添加一个参数，并排在第一位，这个参数就是这个对象本身，约定俗成用 self 指代。谁调用就是谁，可用于在类的内部获取对象属性和对象方法。
```python
>>> class TestClass:
        def say_hello():
            print('你好，')

		
>>> zs = TestClass()
>>> zs.name = '张三'
>>> zs.say_hello()
Traceback (most recent call last):
  File "<pyshell#6>", line 1, in <module>
    zs.say_hello()
TypeError: say_hello() takes 0 positional arguments but 1 was given
>>> 
>>> 
>>> class TestClass:
        def say_hello(self):
            print(self)
            print(type(self))
            print(id(self))
            print(f'你好，我是{self.name}')

		
>>> lisi = TestClass()
>>> wangwu = TestClass()
>>> 
>>> lisi.name = '李四'
>>> wangwu.name = '王五'
>>> 
>>> lisi.say_hello()
<__main__.TestClass object at 0x000001F2EC09F1F0>
<class '__main__.TestClass'>
2142853788144
你好，我是李四
>>> type(lisi);id(lisi)
<class '__main__.TestClass'>
2142853788144
>>> 
>>> wangwu.say_hello()
<__main__.TestClass object at 0x000001F2EC09F970>
<class '__main__.TestClass'>
2142853790064
你好，我是王五
>>> type(wangwu);id(wangwu)
<class '__main__.TestClass'>
2142853790064
```

#### 构造方法与对象属性
查看对象的属性和方法：`dir(o)`
如果想要创建的不同实例化对象在创建时就拥有共同的属性，可以使用构造方法来实现。
构造方法：`def __init__(self): pass`。此方法在创建对象时，由Python自动调用。
可以随时给已创建的对象添加新的属性和方法。同一类的对象也可以拥有不同的属性和方法。

对象访问属性，优先从自己身上找，找到即返回，找不到会按照 \_\_class\_\_ 的值即对应的类中去找。

查看类和对象的属性，可通过 \_\_dict\_\_ 属性来查看所有属性。实例对象的 \_\_dict\_\_ 属性可以修改，类的只读不可修改。
查看对象属性值：对象.属性
修改对象属性值：对象.属性=值
删除对象属性：del 对象.属性
调用对象方法：对象.对象方法()
```python
>>> class A:
        def __init__(self, name, age):
            self.name = name
            self.age = age

		
>>> zs =  A('张三', 18)
>>> zs.name, zs.age
('张三', 18)
>>> 
>>> zs.work = '法外狂徒'
>>> zs.work
'法外狂徒'
>>> 
>>> dir(zs)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'age', 'name', 'work']
>>> zs.__class__
<class '__main__.A'>
>>> zs.__dict__
{'name': '张三', 'age': 18, 'work': '法外狂徒'}
```

#### 私有属性与私有方法
如果想要某属性或方法不能在类的外面被使用，只能在类里面被使用，可以将其变为类的私有属性或私有方法。
具体做法是在该属性或方法的名称前加上两个下划线`__私有属性/方法的名称`，Python会自动将私有属性或方法更名为`_类__私有属性或方法的名称`。

如果在类外面，使用类的实例化对象动态添加与私有属性同名的属性`ob.__属性=值`，注意，这并没有在修改私有属性的值，而是动态添加属性，二者不相干，因为私有属性已被自动化改名了。所以，并不是绝对的私有属性，而是将私有属性自动更名，以致在类的外部使用原名会报错。

但是，用对象调用这个自动更名后的属性`ob._类__私有属性名称`，是可以获取或修改值的，不推荐这种方式，更推荐使用 getter/setter 方法，即通过自定义方法来间接调用私有属性或方法。

getter/setter
若要在类的外部获取或设置私有属性，可先在类里面定义非私有方法返回或设置私有属性，再通过实例化对象调用这个方法即可得到或设置私有属性。
通过setter方法可以限制用户操作，保证数据合规。比如增加判断语句，从而拒绝超过范围的数值。

私有对象属性和方法可通过自定义对象方法来获取和修改。
```python
# 本例中，私有属性 __loc 被自动更名为 _Horse__loc
>>> class Horse:
	def __init__(self, name, speed):
		self.name = name
		self.speed = speed
		self.__loc = 0
			
	def race(self):
		self.__loc += round(self.speed*random.random())
		print(f'{self.name}已经跑到了{self.__loc}米处！')
			
	def get_loc(self):
		return self.__loc
			
	def set_loc(self, x):
		# 可以允许让多少米，最大10米
		if 0 <= x <= 10:
			self.__loc = x
		else:
			print('只能让0到10米')
			
	def __say(self, saysth):
		print(saysth)
		
	def get_say(self, saysth):
		self.__say(saysth)
		
	def set_say(self, saysth):
		self.__say(saysth)
		
>>> chitu = Horse('赤兔', 6)
>>> 
>>> chitu.__loc = 80
>>> 
>>> chitu.set_loc(10)
>>> 
>>> chitu.race()
赤兔已经跑到了14米处！
>>> 
>>> chitu.get_loc()
14
>>> chitu._Horse__loc
14
>>> chitu.__loc
80
>>> chitu.set_loc(11)
只能让0到10米
>>> 
>>> chitu._Horse__loc = 999
>>> chitu.get_loc()
999
>>> chitu._Horse__loc
999
>>> chitu.get_say('hi')
hi
>>> chitu.set_say('hello')
hello
```

#### 类属性与类方法
写在类中、方法外的属性，可被子类继承。
调用：类名.类属性 或 对象.类属性。
修改：类名.类属性 = 值
删除：del 类.类属性
注意：当实例化对象动态添加一个与类属性同名的属性时，这个属性是对象属性，与类属性无关。

```python
>>> class A:
        PI = 3.14159
        
        def __init__(self, name):
            self.name = name

		
>>> a = A('LYF')
>>> a.PI, a.name
(3.14159, 'LYF')
>>> A.PI
3.14159
>>> a.PI = 4.14159
>>> a.PI, A.PI
(4.14159, 3.14159)
>>> 
>>> class B(A):
        pass

>>> b = B('LISI')
>>> b.PI, b.name
(3.14159, 'LISI')

>>> A.PI = 9
>>> A.PI
9
>>> class C(A):
        pass

>>> c = C('CC')
>>> c.PI
9
```

私有类属性可通过自定义类方法来获取和修改。
类方法：类中以 @classmethod 修饰的方法。
```python
>>> class Person(object):
	__country = "中国"

	@classmethod
	def get_country(cls):
		return cls. __country

	@classmethod
	def set_country(cls, country):
		cls. __country = country

		
>>> lyf = Person()
>>> lyf.get_country()
'中国'
>>> lyf.set_country('美国')
>>> lyf.get_country()
'美国'
>>> lyf._Person__country
'美国'
>>> Person.get_country()
'美国'
>>> Person.set_country('中国')
>>> lyf.get_country()
'中国'
```

#### 静态方法
类中以 @staticmethod 修饰的方法。当不用对象方法和类方法时，可用静态方法。
```python
@staticmethod
def hello():

    print("hello")
```

####  None 空对象
```python
>>> def my_func():
        return

>>> i = my_func()
>>> i
>>> type(i)
<class 'NoneType'>
>>> i is None
True
>>> i is not None
False
```

#### 魔法方法

魔法方法 | 说明
---- | ----
\_\_new\_\_ | 监听python使用其类创建对象，并返回对象作为参数给\_\_init\_\_方法使用。创建对象时自动运行。
\_\_init\_\_ | 监听python使用其类创建对象完成，解释器会自动调用该方法，可用于给对象属性初始化赋值。
\_\_str\_\_ | 监听对象属性变化，返回一个字符串，自动调用。一旦设置了该方法，再打印对象print(对象)则是打印该方法的返回值，而不是内存地址。
\_\_del\_\_ | 监听对象销毁（对象的引用记录为0），对象销毁时自动运行，可用于对象销毁时的业务需求。销毁对象：del 对象。

当对象被创建完成，对象的引用计数就 +1，当有变量保存了该引用后，引用计数 +1。
当使用del() 删除变量指向的对象时，则会减少对象的引用计数。如果对象的引用计数不为1，那么会让这个对象的引用计数减1，当对象的引用计数为0的时候，则对象才会被真正删除（内存被回收）。
```python
>>> class Hero(object):
    def __new__(cls, *args, **kwargs):
        return object.__new__(cls)

    def __init__(self, name, age, gender="女"):
        self.name = name
        self.age = age
        self.gender = gender

    def __str__(self):
        return "姓名：%s 年龄：%d 性别：%s" % (self.name, self.age, self.gender)

    def __del__(self):
        print("对象已销毁")

>>> gy = Hero('关羽', 45, '男')
>>> print(gy)
姓名：关羽 年龄：45 性别：男
>>> del(gy)
对象已销毁
```

#### 单例模式类
普通类：不同实例对象分别占用不同的内存地址空间。
单例类：所有实例对象都指向首个对象开辟的内存空间地址，且属性信息不变。
实现思路：1、控制 \_\_new\_\_ 返回的对象每次都是第1个对象，2、控制属性只赋值第1次：第一个对象正常属性赋值，后来的不赋值。

内存地址开辟| 普通类 | 单例模式类
---- | ---- | ----
不同对象 | 不同 | 相同，指向第1个对象地址
同一对象的不同属性 | 不同 | 不同
不同对象的同一属性 | 不同 | 相同，指向第1个对象的属性地址
不同对象的同一方法 | 相同 | 相同

```python
>>> class Person(object):
    __instance = None
    __is_first = True

    def __new__(cls, *args, **kwargs):
        if not cls.__instance:
            cls.__instance = object.__new__(cls)

        return cls.__instance

    def __init__(self, name, age):
        if Person.__is_first:
            self.name = name
            self.age = age
            Person.__is_first = False

    def __str__(self):
        return "%s %d" % (self.name, self.age)

    def eat(self):
        pass

>>> lyf = Person("刘亦菲", 18)
>>> xq = Person("小七", 16)
>>> print(lyf);print(xq)
刘亦菲 18
刘亦菲 18
>>> id(lyf);id(xq)
2829484026320
2829484026320
>>> lyf.name;lyf.age;xq.name;xq.age
'刘亦菲'
18
'刘亦菲'
18
>>> id(lyf.eat());id(xq.eat())
140724699864280
140724699864280
```
### 封装
把同类事物的属性和方法，写到同一段代码（类）中，使得结构清晰。

### 多态
方法：多个类都实现了同一个接口，即同名的方法，所以可以把这些类的实例化对象视作同一种实体，统一调用它们的这个方法。或者反过来说，同一个方法名，在不同的类中执行的具体代码不同，即“同一实体、多种形态”。

如果有很多个对象，每个对象属于不同的类，但是只要这些对象拥有同一个名字的方法，那么，就可以把这些对象视作同一个类型集中在一起，循环遍历每个对象并执行这个同名的方法。核心在于是否有同名的方法。

如能够使用内置函数len()计算字符串或列表的长度，是因为在 str 类和 list 类中，都定义有一个同名的计算长度方法：`def __len__(self)`，而 len() 中使用参数对象调用了这个方法。
```python
>>> len('abcde');len(['a', 'b', 'c'])
5
3
>>> 'abcde'.__len__()
5
>>> ['a', 'b', 'c'].__len__()
3
>>> '__len__' in dir(str)
True
>>> '__len__' in dir(list)
True


# 前提：Horse 和 Funs 这两个类同时拥有get_image()、get_location()和run()方法，即使方法的具体执行代码不同。
class Horse:
    def __init__(self, name, speed):
        pass

    def run(self):
        pass

    def get_image(self):
        pass

    def get_location(self):
        pass

class Funs:
    def __init__(self, img_file, x, y, up):
        pass
        self.__y = y
        self.__up = up

    def run(self):
        if self.__up:
            self.__y -= 30
        else:
            self.__y += 30
        self.__up = not self.__up

    def get_image(self):
        pass

    def get_location(self):
        pass

horse_list = [Horse('赤兔', 6), Horse('绝影', 5), Horse('乌骓', 7), Funs('x.png', 100, 300, True), Funs('x2.png', 300, 300, False)]

for h in horse_list:
    screen.blie(h.get_image(), h.get_location())
    h.run()
```

### 继承
当两个类拥有共同的属性和方法时，为避免写重复的代码，可以引入继承，让一个类继承另一个类，如此形成了父类和子类的关系。一个子类可以继承多个父类，即多继承。

在 Python 3 中，object 为所有类的基类，所有类在创建时默认继承 object，所以不声明继承 object 也可以。

继承写法
单继承：class 类名(父类名): code
多继承：class 类名(父类1, 父类2, ...): code

继承规则
父类的所有公共属性和方法可以被继承，私有属性和方法不可被继承。
子类重写：子类同时可以拥有自己独特的属性和方法，也可以对继承自父类的不满意的属性和方法进行重写处理。

子类实例化对象时，会先在子类里找自己的创建方法，找到则执行，找不到就会到父类中去找，找到则动态创建属性。

子类对象调用父类：子类对象.父类属性或方法
多继承中，调用父类会首先找第一个父类中有没有，有则执行，不再继续向下寻找，没有则找下一个父类，直到找完所有父类。

子类调用父类
子类调用父类中同名的属性和方法：自定义子类方法
父类名.父类方法(self)：推荐使用，适用于多继承。
super().父类方法()
super(子类名, self).父类方法()
super()在多继承中指的是第一个父类，适合单继承。

#### 方法的继承
子类继承了父类的方法，同时子类也能拥有自己方法，实现了对父类的扩展，子类还能对继承自父类的方法进行覆写（保留方法名但更改功能代码），使之符合子类的特征。

方法的继承与多态：子类和父类由于拥有同名的方法，所以符合多态。
子类的实例化对象属于本身类，也属于父类。判断对象是否属于一个类，可用内置函数：`isinstance(obj, class)`。
```python
>>> class A:
        def __init(self):
            self.money = 10000
            self.house = 2
        
        def my_small_goal(self):
            print("走，先去赚他一个亿！")

		
>>> class B(A):
        def my_small_goal(self):
            print("世界那么大，我要去看看。")

		
>>> class C(A):
        def my_small_goal(self):
            print("我就是要修仙，修他个一万年。")

		
>>> class D(A):
    pass

>>> ob_list = [A(), B(), C(), D()]
>>> for ob in ob_list:
    	ob.my_small_goal()

	
走，先去赚他一个亿！
世界那么大，我要去看看。
我就是要修仙，修他个一万年。
走，先去赚他一个亿！
>>> 
>>> a, b = A(), B()
>>> isinstance(a, B);isinstance(b, A);isinstance(b, C)
False
True
False
```

#### 属性的“继承”
子类能够使用与父类属性同名的属性，不是因为继承了父类的属性，而是继承了父类的 init 创建方法，动态创建相同名称的属性。

子类如果既想要父类的属性，也想要有自己独特的属性，可以覆写 init 构建方法。
在子类中调用父类的方法：`super().方法()` 或 `父类名.方法(self)`。例如，调用父类的构建方法可以用：`super().__init__()` 或 `父类名.__init__(self)`。
当子类的实例对象属性比父类多，在处理二者相同的属性时，可考虑在 \_\_init\_\_ 中调用父类的 \_\_init\_\_ 方法，\_\_str\_\_ 中也可调用。
```python
>>> class A:
        def __init__(self):
            self.p = 'hello'

		
>>> class B(A):
    	pass

>>> b = B()
>>> b.p
'hello'

>>> class C(A):
        def __init__(self):
            self.q = 'bye'

		
>>> c = C()
>>> c.q
'bye'
>>> c.p
Traceback (most recent call last):
  File "<pyshell#15>", line 1, in <module>
    c.p
AttributeError: 'C' object has no attribute 'p'

>>> class D(A):
        def __init__(self):
            super().__init__()
            # A.__init__(self)
            self.q = 'bye'

		
>>> d = D()
>>> d.p, d.q
('hello', 'bye')
```

#### 私有属性的“继承”
私有属性不能“继承”是个伪命题，因为连真正的私有属性都没有，都是通过自动改名字实现的。
```python
>>> class A:
        def __init__(self, name, money):
            self.name = name
            self.__money = money
        def make_friend(self):
            print(f'{self.name}说，这些钱拿去花：{self.__money}')

		
>>> class B(A):
        def make_money(self):
            print(f'{self.name}说，这些钱都是我的：{self.__money}')

		
>>> b = B('张三', 100)
>>> b.name, b._A__money
('张三', 100)
>>> b.make_friend()
张三说，这些钱拿去花：100
>>> 
>>> b.make_money()
Traceback (most recent call last):
  File "<pyshell#14>", line 1, in <module>
    b.make_money()
  File "<pyshell#10>", line 3, in make_money
    print(f'{self.name}说，这些钱都是我的：{self.__money}')
AttributeError: 'B' object has no attribute '_B__money'
>>> 
>>> class C(A):
        def make_money(self):
            print(f'{self.name}说，这些钱都是我的：{self._A__money}')

		
>>> c = C('李四', 200)
>>> c.make_money()
李四说，这些钱都是我的：200
```

#### 多继承
子类可以继承多个父类，class A(B, C)。与单继承一样，子类也继承了各个父类的方法，也可以对方法进行覆写。不推荐使用。
查找同名方法，先在子类中找，找不到再到父类中找，多继承则按父类的编写顺序从前往后找。
```python
>>> class Person(object):
    country = "中国"

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return "%s %d" % (self.name, self.age)

    def eat(self):
        print("能吃")

    def act(self):
        print("会cosplay")

>>> class Star(object):
    gender = "女"

    def act(self):
        print("会演戏")

    def sing(self):
        print("会唱歌")

>>> class MovieStar(Person, Star):
    works = "《花木兰》"

    def __init__(self, name, age, gender):
        Person.__init__(self, name, age)
        # self.name = name
        # self.age = age
        self.gender = gender

    def __str__(self):
        str = Person.__str__(self)
        return str + " %s" % self.gender
        # return "%s %d %s" % (self.name, self.age, self.gender)

    def act(self):
        print("会演电影")

    def act2(self):
        Star.act(self)
        Person.act(self)
        # super().act()
        # super(MovieStar, self).act()
        # print(super().gender)

>>> lyf = MovieStar("刘亦菲", 18, "女")
>>> lyf.name;lyf.age;lyf.gender
'刘亦菲'
18
'女'
>>> lyf.country;lyf.works
'中国'
'《花木兰》'
>>> lyf.eat()
能吃
>>> lyf.act()
会演电影
>>> lyf.act2()
会演戏
会cosplay
```

### 面向对象的消息机制
一个对象可以作为其它对象的属性，也可以作为参数传递给函数或其它对象的方法。

案例：养盅游戏
main.py、role.py、player.py

## 函数式编程
 函数的目的，就是减少重复书写，使代码一次书写、反复调用。
 将一个函数作为另一个函数的参数，就是函数式编程，即使用高阶函数。
 
### 高阶函数
 由于函数名是个变量，指向了函数的内存地址，所以，可以使用函数名将一个函数作为另一个函数的参数。拥有函数作为参数的函数，就是高阶函数。
 比如  sorted(iterable, key=None)、filter(function or None, iterable)、map(function, \*iterables) 、functools.partial(func, \*args, \*\*kws)、functtools.reduce()、lambda表达式等。

### 递归
在函数内部代码中调用自身，就是递归。适用于“层次的分叉树形”结构。比如计算阶乘、字符串反转、斐波那契数列、汉诺塔、科赫曲线、路线选择。

递归看上去是函数自己调用了自己，实际上在内存中是调用了自己的一个“拷贝”（严格地说，是指针和“调用栈”），即另一个“函数”。

难题为递归所破歌：大事化小，小事化了，层层递进，回归提交。

递归限制
```python
>>> import sys
>>> sys.getrecursionlimit()
1000
>>> sys.setrecursionlimit(3000)
>>> sys.getrecursionlimit()
3000
>>> sys.setrecursionlimit(1000)
>>> sys.getrecursionlimit()
1000
```
 
写递归要点：函数 + 分支（基例 + 链条）
计算阶乘
```python
>>> def f(n):
        if n == 1:
            return 1
        else:
            return n * f(n-1)

	
>>> f(5)
120

>>> def f(n):
        r = 1
        for i in range(1, n+1):
            r *= i
        return r

>>> f(5)
120

>>> n = 5
>>> reduce(lambda x, y: x*y, range(1, n+1))
120
```

计算第几个 fibonacci 数
```python
>>> def fib(n):
        if n == 1 or n == 2:
            return 1
        else:
            return fib(n-1) + fib(n-2)

	
>>> fib(10)
55
>>> list(map(fib, range(1, 11)))[-1]
55
```

字符串反转
```python
>>> def str_reverse(s):
        if s == '':
            return ''
        else:
            return s[-1] + str_reverse(s[:-1])

	
>>> str_reverse('abc')
'cba'
```

多维列表变一维列表
```python
>>> x = [5, [2, 3,['a', 'b']], [[5, 8],[6, [9, 10]]]]
>>> 
>>> def perspective(x, r):
        for e in x:
            if not isinstance(e, list):
                r.append(e)
        else:
            perspective(e, r)
        return r

>>> perspective(x, [])
[5, 2, 3, 'a', 'b', 5, 8, 6, 9, 10]

>>> def perspective(x):
        for e in x:
            if not isinstance(e, list):
                print(e)
        else:
            perspective(e)
        return

>>> perspective(x)
5
2
3
a
b
5
8
6
9
10
```

### 装饰器
函数的返回值，也可以是函数。
函数的内部，也可以定义新函数。

单例模式类
数据驱动

# 模块应用
## 模块总览表
标准模块表：

| 模块 | 说明 |
| ---- | ---- |
| [[this]] | 打印 python 之禅。 |
| [[os]] ||
| [[re]] | 正则表达式 |
| [[random]] | 随机数 |
| [[turtle]] | 画图 |

第三方模块表：

| 模块 | 说明 |
| ---- | ---- |
| [[tushare]] | 股票 |
| [[wordcloud]] | 词云 |
| [[jieba]] | 中文分词 |
| [[pinyin]] | 汉字转拼音 |
| [[pyinstaller]] | 打包 .py 文件成可执行文件 |

应用方向与模块映射：

| 应用方向 | 标准模块 | 第三言模块 |
| --- | --- | --- |
| 软件自动化测试 | [[unittest]] | [[selenium]], [[pytest]], [[hytest]], [[scapy]] |
| 网页请求/爬虫 | [[urllib]]  | [[requests]], [[bs4]] |
| 图形化 GUI | [[tkinter]] | |
| 数据文件 | [[json]] | [[xlwings]] |
| 数据分析 | | [[pandas]] |
| 数据可视化 | | [[matplotlib]], [[pygal]] |
| 游戏 | [[pygame]] | |
| 数学 | [[decimal]], [[math]] | [[sympy]], [[numpy]] |
| 日期时间计算 | [[time]], [[datetime]]  | [[dateutil]], [[arrow]], [[workalender]] |

## 日期时间计算
时间戳参考：[[time#^57812c|time.time()]]

日期相减的结果是时间差 timedelta 对象，可以计算相差几天零几秒；
日期不能直接相加，但可以用日期加时间差对象，求得未来时间点。
时间差参考：[[datetime#datetime timedelta]]和[[dateutil#时间差：relativedelta]]。

## 精算
参考：[[数据类型#浮点数运算]]

## 容器自定义规则排序：列表
| 函数/方法 | 说明 |
| --- | --- |
| list.sort(key=None, reverse=False) | 对原列表进行排序 |
| sorted(iterable, key=None, reverse=False) | 内置排序函数，原列表不变 |
| | key=函数名 或 lambda 表达式 |
| random.shuffle(list) | 对列表随机排序 |

key参数说明：指定 key=函数名，没有括号，排序时就会先将列表中每个元素分别代入该函数，得到返回结果；然后根据返回结果的大小，对列表元素排序。

案例：读取 Excel 表格内容，进行排序。
```python
>>> import xlwings as xw
>>> app = xw.App()
>>> wb = app.books.open(r"C:\Users\yi\Desktop\degree.xlsx")
>>> degree_list = wb.sheets['Sheet1'].range('B3:C16').value
>>> wb.close()
>>> app.quit()
>>> 
>>> degree_list
[['张三', '硕士'], ['李四', '学士'], ['王五', '壮士'], ['赵六', '博士'], ['田七', '圣斗士'], ['刘八', '学士'], ['崔九', '学士'], ['杜十娘', '学士'], ['萧十一郎', '博士'], ['钱十二', '硕士'], ['春十三娘', '学士'], ['郑十四', '学士'], ['冯十五', '硕士'], ['朱重八', '壮士']]
>>> degree_list_bak = degree_list.copy()
>>> 
>>> def sort_degree(li):
    	t = li[1]
    	if t == '学士':
    		return 0
    	elif t == '硕士':
    		return 1
    	elif t == '博士':
    		return 2
    	elif t == '壮士':
    		return 3
    	elif t == '圣斗士':
    		return 4
    	else:
    		return -1
>>> 
>>> degree_list_bak.sort(key=sort_degree, reverse=True)
>>> degree_list_bak
[['田七', '圣斗士'], ['王五', '壮士'], ['朱重八', '壮士'], ['赵六', '博士'], ['萧十一郎', '博士'], ['张三', '硕士'], ['钱十二', '硕士'], ['冯十五', '硕士'], ['李四', '学士'], ['刘八', '学士'], ['崔九', '学士'], ['杜十娘', '学士'], ['春十三娘', '学士'], ['郑十四', '学士']]
>>> 
>>> sorted(degree_list, key=sort_degree, reverse=True)
[['田七', '圣斗士'], ['王五', '壮士'], ['朱重八', '壮士'], ['赵六', '博士'], ['萧十一郎', '博士'], ['张三', '硕士'], ['钱十二', '硕士'], ['冯十五', '硕士'], ['李四', '学士'], ['刘八', '学士'], ['崔九', '学士'], ['杜十娘', '学士'], ['春十三娘', '学士'], ['郑十四', '学士']]
>>> 
>>> sorted(degree_list, key=lambda e: e[1])
[['赵六', '博士'], ['萧十一郎', '博士'], ['田七', '圣斗士'], ['王五', '壮士'], ['朱重八', '壮士'], ['李四', '学士'], ['刘八', '学士'], ['崔九', '学士'], ['杜十娘', '学士'], ['春十三娘', '学士'], ['郑十四', '学士'], ['张三', '硕士'], ['钱十二', '硕士'], ['冯十五', '硕士']]
>>> 
>>> 
>>> degree_dict = {'学士': 0, '硕士': 1, '博士': 2, '壮士': 3, '圣斗士': 4}
>>> degree_list_bak.sort(key=lambda e: degree_dict[e[1]])
>>> degree_list_bak
[['李四', '学士'], ['刘八', '学士'], ['崔九', '学士'], ['杜十娘', '学士'], ['春十三娘', '学士'], ['郑十四', '学士'], ['张三', '硕士'], ['钱十二', '硕士'], ['冯十五', '硕士'], ['赵六', '博士'], ['萧十一郎', '博士'], ['王五', '壮士'], ['朱重八', '壮士'], ['田七', '圣斗士']]
>>> sorted(degree_list, key=lambda e: degree_dict[e[1]])
[['李四', '学士'], ['刘八', '学士'], ['崔九', '学士'], ['杜十娘', '学士'], ['春十三娘', '学士'], ['郑十四', '学士'], ['张三', '硕士'], ['钱十二', '硕士'], ['冯十五', '硕士'], ['赵六', '博士'], ['萧十一郎', '博士'], ['王五', '壮士'], ['朱重八', '壮士'], ['田七', '圣斗士']]
>>> 
>>> 
>>> sorted([5, 0, 21, -2, 15, 2, -5], key=abs)
[0, -2, 2, 5, -5, 15, 21]
```

## 构造姓名
```python
>>> import random
>>> 
>>> xingshi = ['刘', '韩', '南宫', '陈', '辛', '厉']
>>> mingzi = ['亦', '菲', '立', '婉', '巧', '倩', '如', '音', '飞', '羽']
>>> for e in zip(xingshi, mingzi):
	print(''.join(e))

	
刘亦
韩菲
南宫立
陈婉
辛巧
厉倩
>>> names = []
>>> mingzi *= 2
>>> while len(names) < 20:
        name = ''.join(random.sample(xingshi, 1) + random.sample(mingzi, random.randint(1, 2)))
        if name not in names:
            names.append(name)

		
>>> names
['厉菲音', '韩立立', '陈亦音', '陈倩巧', '厉如', '南宫音', '刘婉菲', '韩亦婉', '陈音亦', '辛婉飞', '厉巧立', '韩倩', '韩立婉', '南宫飞飞', '南宫飞音', '辛倩巧', '刘婉', '韩如音', '韩如倩', '厉婉']
```

## 网络爬虫
```
爬虫：向服务器发送请求，获得响应，对响应进行信息提取。按尺寸可分为爬取网页、网站、全网。
引发的问题：服务器性能骚扰、版权内容的法律风险、隐私泄露
反制措施：/robots.txt协议公告声明（User-agent, Disallow）、对请求来源审查（User-agent）。

信息标记：XML、JSON、YMAL
信息提取：标记解析/搜索

节点元素：标签、文本
	
\r 可设置打印时光标移至当前行的行首位置。
```