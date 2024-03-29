---
tags: re 正则表达式
---

re，Regular Expression，正则表达式，是一种定义想要查找的文本内容的格式特征或文字模式的语言，内置模块。

注意细节：
1，表达式中每个字符（包括元字符），只能匹配文本中的一个字符。
2，量词只能指定前面相邻的一个字符（或组）的连续出现次数。
3，各个匹配结果之间绝无重叠。
4，默认贪婪匹配模式。

```Python
>>> import re
>>> 
>>> p = re.compile(r'\d+')
>>> 
# 各个匹配结果之间绝无重叠
>>> p.findall('\d\d\d', '123456789')
['123', '456', '789']
```
# 函数

| 函数/方法 | 说明 |
| ---- | ---- |
| re.compile(pattern, flags=0) | 根据正则式创建一个Pattern对象，之后需要使用该正则式时，直接使用该对象处理即可。|
| re.findall(pattern , s, flags=0) | 查找，以字符串列表返回所有结果，但若有捕获组，则只返回捕获组。否则返回空列表。|
| re.sub(pattern, repl, s, count=0, flags=0) | 查找并替换。|
| re.search(pattern, s, flags=0) | 查找，以Match对象返回首个匹配项，否则返None。|
| re.match(pattern, s, flags=0) | 查找，以Match对象返回首个匹配项，首字符必须匹配，否则返None。可判断输入是否符合预期。|
| Match.group() | 获取Match对象的完整结果及捕获组。group(0)返回完整匹配结果，group(n)返回第n个捕获组。|
| Match.groups() | 以元组形式返回Match对象的所有捕获组。|
| re.finditer(pattern , s, flags=0) | 查找，返回一个迭代器对象（包含所有匹配项、每一匹配项的完整字符串及捕获组）。|

查找函数优缺点比较

| 查找函数 | 优点 | 缺点 |
| ---- | ---- | ---- |
| re.findall() | 能够返回所有匹配结果。 | 在有捕获组的情况下，只返回捕获组，不能够返回匹配的完整子串。 |
| re.search()、re.match() | 既能返回匹配的完整子串，又能返回捕获组。 | 只能返回第一个匹配结果。 |
| re.finditer() | 结合了上述二者的优点，既能返回所有匹配结果，又能返回完整子串，还能返回捕获组。| |

```Python
# 判断用户输入：由小写字母、数字、下划线构成，下划线不能开头
re.match(r'^[a-z\d][a-z\d_]*$', input('please input: '))


>>> s = '25-37-8888,21-05-7777'
>>> 
>>> re.findall(r'(\d+)-(\d+)-(\d+)', s)
[('25', '37', '8888'), ('21', '05', '7777')]
>>> 
>>> re.search(r'(\d+)-(\d+)-(\d+)', s)
<re.Match object; span=(0, 10), match='25-37-8888'>
>>> 
>>> re.match(r'(\d+)-(\d+)-(\d+)', s)
<re.Match object; span=(0, 10), match='25-37-8888'>
>>> 
>>> print(re.match(r'(\d+)-(\d+)-(\d+)', 'a' + s))
None
>>> 
>>> t = re.search(r'(\d+)-(\d+)-(\d+)', s)
>>> t.group(), t.group(0), t.group(1), t.group(2), t.group(3)
('25-37-8888', '25-37-8888', '25', '37', '8888')
>>> t.groups()
('25', '37', '8888')
```

re.finditer()，返回一个迭代器对象，里面包含了所有匹配结果中每一个结果的 Match 对象。
循环该迭代器对象，并对其中的 Match 对象调用 Match.group() 和 Match.groups() 方法，即可得到所有匹配结果的完整子串和捕获组。
```Python
>>> s = '25-37-8888,21-05-7777'
>>> 
>>> re.finditer(r'(\d+)-(\d+)-(\d+)', s)
<callable_iterator object at 0x00000214C16AF970>
>>> a = re.finditer(r'(\d+)-(\d+)-(\d+)', s)
>>> for e in a:
        e

	
<re.Match object; span=(0, 10), match='25-37-8888'>
<re.Match object; span=(11, 21), match='21-05-7777'>
>>> for m in a:
        print(m.group(0), m.groups())

	
>>> a = re.finditer(r'(\d+)-(\d+)-(\d+)', s)
>>> for m in a:
        print(m.group(0), m.groups())

	
25-37-8888 ('25', '37', '8888')
21-05-7777 ('21', '05', '7777')
```

re.compile()
在需要多次重复使用同一正则式时，由于正则式已经编译，所以不用重复书写，且速度极快。返回的对象也能调用re的匹配函数。
```Python
>>> s = '25-37-8888,21-05-7777'
>>> 
>>> p = re.compile(r'\d+')
>>> 
>>> p.findall(s)
['25', '37', '8888', '21', '05', '7777']
>>> 
>>> p.match(s)
<re.Match object; span=(0, 2), match='25'>
>>> 
>>> p.search(s)
<re.Match object; span=(0, 2), match='25'>
>>> 
>>> p.finditer(s)
<callable_iterator object at 0x00000214C163EE50>
>>> for i in p.finditer(s):
        print(i)

	
<re.Match object; span=(0, 2), match='25'>
<re.Match object; span=(3, 5), match='37'>
<re.Match object; span=(6, 10), match='8888'>
<re.Match object; span=(11, 13), match='21'>
<re.Match object; span=(14, 16), match='05'>
<re.Match object; span=(17, 21), match='7777'>
```
# 匹配模式

| 常用匹配模式 | 简写 | 含义与用途 |
| ---- | ---- | ---- |
| re.ASCII | re.A | 让\\w,\\W,\\b,\\B,\\d,\\D,\\s,\\S<br>只匹配ASCII，不匹配Unicode字符（比如中文)。 |
| re.IGNORECASE | re.I | 忽略大小写。 |
| re.MULTILINE | re.M | 多行模式，^和$代表每一行的行首行尾位置。 |
| re.DOTALL | re.D | 单行模式，点号“.”可以匹配换行符。 |

## 贪婪匹配
贪婪匹配：匹配时当有多种方案供选择时，选择最长。默认匹配模式。

下面的例子中，采用'\\d+'可以有多种匹配方案（\['1', '0', '0'\]、\['10', '0'\]、\['1', '00'\]、\['100'\]），因为贪婪匹配，则结果为\['100'\]。
```Python
>>> import re
>>> 
>>> re.findall('\d', '100')
['1', '0', '0']
>>> 
>>> re.findall('\d+', '100')
['100']
>>> 
>>> re.findall('\w+\d', 'abc256def8xyz')
['abc256def8']
```

## 懒惰匹配
懒惰匹配：匹配时当有多种方案供选择时，选择最短。
使用方法：在量词（\*、?、+、{}）后面加上问号‘?’，即为懒惰匹配。
```Python
>>> re.findall('\w+?\d', 'abc256def8xyz')
['abc2', '56', 'def8']
>>> 
>>> re.findall('\w?\d', 'abc256def8xyz')
['c2', '56', 'f8']
>>> re.findall('\w??\d', 'abc256def8xyz')
['c2', '5', '6', 'f8']
```

## 单行、多行模式
多行模式下，允许^$分别代表每一行的行首行尾。
re.findall(regex, s, re.MULTILINE)

单行模式下，允许点号"."代表换行符。
re.findall(regex, s, re.DOTALL)

同时使用单行、多行模式
re.findall(regex, s, re.DOTALL|re.MULTILINE)

# 字符
## 按“字面意思"的普通字符
普通字符：按字面值理解
0：代表0

## 按”类型“的普通元字符
\\d：代表一个数字字符，即0~9。
\\D：代表不是数字的任意一个字符，相当于 \[^0-9\]。
\\s：代表一个空白字符，如空格、换行、缩进等。
\\S：代表一个非空白字符。
\\w：代表一个单词字符，即字母、数字、汉字等可构成词句的字符，不包括标点、空格。
\\W：代表一个非单词字符。
.：点号，代表一个任意字符（是否代表换行符，要看是否设置了“单行模式”）。使用`[\s\S]`也可代表一个任意字符。
```Python
# 开启单行模式
re.findall(regex, s, re.DOTALL)
```

## 位置元字符
\\b：代表一个位置，而不是一个字符，又称“单词分界线”，即一边是文字字符，另一边是非文字字符（如空格）的位置。
‘\\b'在python字符串中，表示退格键。所以要想在正则式中使用'\\b'代表单词分界线，需要在其之前加上’\\‘，即'\\\b'。或者用'r'表示原字符串，失效字符串中大部分转义。
```Python
>>> re.findall('\\b\w+\\b', "John's father is Bob's brother.")
['John', 's', 'father', 'is', 'Bob', 's', 'brother']
>>> 
>>> re.findall(r'\b\w+\b', "John's father is Bob's brother.")
['John', 's', 'father', 'is', 'Bob', 's', 'brother']
>>> 
>>> print('\\b\w+\\b')
\b\w+\b
```

^：多行模式下表示行首，基他模式下表示字符串开头，或整个文本的开头即首行行首。
\$：多行模式下表示行尾，其他模式下表示字符串结尾，或整个文本的末尾，即末行行尾。不包括每行行尾都有的'\\n'换行。
所以，写正则式时要加上行尾的换行：`^$\n`
```Python
>>> s = '''柳宗元 江雪
	千山鸟飞绝， 万径人踪灭。
	孤舟蓑苙翁， 独钓寒江雪。

	《江雪》这首诗作于柳宗元谪居永州期间（805年—815年）。

	孤舟蓑苙翁，独钓寒江雪。在下着大雪的江面上，一叶小舟，一个老渔翁，独自在寒冷的江心垂钓。'''
>>> 
>>> regex = r'^\s*\w{5}，\s*\w{5}。\s*$'
>>> re.findall(regex, s)
[]
>>> re.findall(regex, s, re.MULTILINE)
['\t千山鸟飞绝， 万径人踪灭。', '\t孤舟蓑苙翁， 独钓寒江雪。\n']
>>> re.findall(regex, s, flags=re.MULTILINE)
['\t千山鸟飞绝， 万径人踪灭。', '\t孤舟蓑苙翁， 独钓寒江雪。\n']
>>> 
>>> regex = r'(?:^\s*\w{5}，\s*\w{5}。\s*$\n)+'
>>> re.findall(regex, s, re.MULTILINE)
['\t千山鸟飞绝， 万径人踪灭。\n\t孤舟蓑苙翁， 独钓寒江雪。\n\n']
>>> print(re.findall(regex, s, re.MULTILINE)[0])
	千山鸟飞绝， 万径人踪灭。
	孤舟蓑苙翁， 独钓寒江雪。


```

## 分支选择
分支选择：|
典型用法：regex1 | regex2
```Python
>>> s = '''A公司，发货5000台，联系人：张三，电话 010 -   88223343，邮箱zs@a.com
B公司：联系人：李四，电话：0418-5566778，邮箱：lisi@b.com
C公司发货200台，联系人：王五，电话是（024）66667777，邮箱：wangwu@c.net
D公司电话联系，联系人：赵六，电话0520-33227788，邮箱是 zhaoliu@d.com.
E公司联系人：田七，电话 01083232563，邮箱 tiangqi@163.com
F公司，联系人：John Willams .电话 （001）33243334，邮箱 johnw@mycom.com
G公司，联系人：Ken Wang. 电话（001）56523333，邮箱 778_kenwang@vip.163.com'''
>>> 
>>> regex = r'[\da-z_]+@(?:[\da-z_]+.)+(?:com|net)'
>>> re.findall(regex, s)
['zs@a.com', 'lisi@b.com', 'wangwu@c.net', 'zhaoliu@d.com', 'tiangqi@163.com', 'johnw@mycom.com', '778_kenwang@vip.163.com']
```

## 环视
环视，look around，在找到符合搜索条件的疑似匹配结果后，先观察其前文和后文是否也符合要求，一切都符合，则作为匹配结果。

环视分类：
1，顺序肯定型：(?=后面必须是什么文字)
2，顺序否定型：(?!后面禁止是什么文字)
3，逆序肯定型：(?<=前面必须是什么文字)
4，逆序否定型：(?<!前面禁止是什么文字)

注意：由于逆序回溯检查的效率很低，所以Python正则式要求逆序环视（包括肯定和否定）中，匹配字符的个数应当明确，不能使用\*、+等不确定个数的量词。
```Python
>>> s = '''2014年~2018年，结婚登记对数从170027对降至137818对，而离婚登记对数从56192对增长至66616对。2014年~2018年连续5年的离结比分别为：33.05%、43.97%、58.71%、48.18%、48.34%。
黑龙江的离婚结婚比高达63%。这意味着有100对夫妻结婚，就有63对夫妻离婚，也就是0.63的离结比。上海和北京分别为49%、48%。'''
>>> 
>>> re.findall('[.\d]+%', s)
['33.05%', '43.97%', '58.71%', '48.18%', '48.34%', '63%', '49%', '48%']
>>> 
>>> re.findall('[.\d]+(?=%)', s)
['33.05', '43.97', '58.71', '48.18', '48.34', '63', '49', '48']
>>> 
>>> re.findall('[.\d]+(?![.\d%])', s)
['2014', '2018', '170027', '137818', '56192', '66616', '2014', '2018', '5', '100', '63', '0.63']
>>> 
>>> re.findall('(?<=\.)\d+', s)
['05', '97', '71', '18', '34', '63']
>>> re.findall('(?<=\.)\d+(?=%)', s)
['05', '97', '71', '18', '34']
>>> 
>>> re.findall('(?<![.\d])\d+(?![.\d])', s)
['2014', '2018', '170027', '137818', '56192', '66616', '2014', '2018', '5', '63', '100', '63', '49', '48']
```

# 字符组
## 方括号的自定义字符组
\[\]：方括号设定自定义字符组，代表找一个且必须是方括号内的字符。如\[a甲1\] 表示找一个字符，它或是 'a'，或是'甲'，或是'1'。

如果字符组内的字符是连续的，可以使用连字符'-'。如\[0-9\]、\[a-z\]、\[A-Z\]，方括号内可以将多个连续范围放在一起，如\[2-8c-mA-U\]。

注意：
字符组中的.\*+?{}()符号，都代表普通字符，没有“量词”等特殊含义。
如果想在字符组中表示减号'-'，应当将其放在首位或末位，否则会被认为是范围标记。如\[-，。；（）.\]。

```Python
>>> s = 'a010-3662893,b 027-43123543.c000 - 3233'
>>> 
>>> re.findall('[0-9]', s)
['0', '1', '0', '3', '6', '6', '2', '8', '9', '3', '0', '2', '7', '4', '3', '1', '2', '3', '5', '4', '3', '0', '0', '0', '3', '2', '3', '3']
>>> 
>>> re.findall('[3-7]', s)
['3', '6', '6', '3', '7', '4', '3', '3', '5', '4', '3', '3', '3', '3']
>>> 
>>> re.findall('[0-9]+', s)
['010', '3662893', '027', '43123543', '000', '3233']
>>> 
>>> re.findall('[3-5a-z]', s)
['a', '3', '3', 'b', '4', '3', '3', '5', '4', '3', 'c', '3', '3', '3']
```

## 圆括号的组重复
字符重复，就是字符后面接量词。
组重复，就是组后面接量词，组的格式结构重复指定次数。指定组用圆括号()。

圆括号()，即能代表分组，又能代表一个捕获组。有几对捕获组圆括号，就能得到几对捕获结果。在一个匹配结果里，即使一对括号匹配多段内容，也只保留其中的最后一段，作为这个捕获组的结果。

若想要圆括号()只代表分组，需要使用 (?:)。

(abc)+ 表示分组结构重复至少一次，即能匹配 abc、abcabc。还能表示一个捕获组。
(?:abc)+ 仅表示分组结构重复，不能表示捕获组。
```Python
>>> re.findall('(?:abc)+', 'abc1abcabc2fdafabc3ef')
['abc', 'abcabc', 'abc']
>>> re.findall('(abc)+', 'abc1abcabc2fdafabc3ef')
['abc', 'abc', 'abc']
>>> 
>>> re.findall('(?:abc)+', 'abcabcabcabc')
['abcabcabcabc']
>>> re.findall('(abc)+', 'abcabcabcabc')
['abc']
```

# 反义
^：否定取反。如\[^0-9\] 代表一个不是数字的任意字符。
必须放在组中的第1位，才是否定取反，放在其他位置只是字面字符。

常见用途：
1，用于查找符号A到符号B之间的内容：A\[^B\]+B，即以A开头、然后连续多个B之外的字符，直到遇见一个B结束。
2，用于指定多个分割符分割字符串：\[^，。；？“”,！、.;\]+

# 量词
{N}：将花括号前面的一个字符出现 N 次。
{m, n}：前面的一个字符连续出现，至少 m 次，至多 n 次。
{m, }：前面的一个字符连续出现，至少 m 次。

+：相当于 {1,}，x+ 表示字符x连续出现至少1次。
?：相当于 {0,1}，可有可无，有的话只能出现1次。x? 表示字符x不出现或只出现1次。
\*：相当于 {0,}，出现任意次数。x\* 表示字符x不出现或出现任意次数。

\\d{8}：代表8个类型是数字的字符，即8个任意数字。
\\d+：至少1次连续出现的类型是数字的字符，任意数字。
\\d?：表示一个数字类型的字符，或空字符串。
\\w+：表示由连续文字字符构成的文本
```Python
>>> s = 'a010-3662893,b 027-43123543.c000 - 3233'
>>> 
>>> re.findall('\d{1,}-\d{1,}', s)
['010-3662893', '027-43123543']
>>> 
>>> re.findall('\d{1,} {0,}- {0,}\d{1,}', s)
['010-3662893', '027-43123543', '000 - 3233']
>>> 
>>> re.findall('\d+ ?- ?\d+', s)
['010-3662893', '027-43123543', '000 - 3233']
>>> 
>>> re.findall('\d+ *- *\d+', s)
['010-3662893', '027-43123543', '000 - 3233']
>>> 
>>> re.findall('\d?', s)
['', '0', '1', '0', '', '3', '6', '6', '2', '8', '9', '3', '', '', '', '0', '2', '7', '', '4', '3', '1', '2', '3', '5', '4', '3', '', '', '0', '0', '0', '', '', '', '3', '2', '3', '3', '']
>>> 
>>> re.findall('\d+\s*-\s*\d+', s)
['010-3662893', '027-43123543', '000 - 3233']
>>> 
>>> re.findall('\w', s)
['a', '0', '1', '0', '3', '6', '6', '2', '8', '9', '3', 'b', '0', '2', '7', '4', '3', '1', '2', '3', '5', '4', '3', 'c', '0', '0', '0', '3', '2', '3', '3']
>>> re.findall('\w+', s)
['a010', '3662893', 'b', '027', '43123543', 'c000', '3233']
>>> re.findall('0\w+', s)
['010', '027', '000']
>>> 
>>> s = 'a联系人：张三 ，b联系人： 李四，c联系人：liu yifei'
>>> re.findall('联系人：\s*\w+\s*\w+', s)
['联系人：张三', '联系人： 李四', '联系人：liu yifei']
```

 # 捕获组
正则表达式中指定要捕获的局部信息，就是捕获组，用半角圆括号()表示。

使用捕获组匹配后返回的结果中，只包含捕获组的内容，而不包含其他位置的匹配内容。如果正则式只含一个捕获组，则结果列表中每一项都是一个字符串，即该捕获组的内容；如果正则式有多个捕获组，则结果列表中每一项都是一个元组，其中包含各个捕获组内容。
```Python
>>> s = 'a010-3662893,b 027-43123543.c000 - 3233'
>>> regex = '(\d+)[-\s]*(\d+)'
>>> re.findall(regex, s)
[('010', '3662893'), ('027', '43123543'), ('000', '3233')]
>>> re.findall('(\d+)[-\s]*\d+', s)
['010', '027', '000']
```

# 转义
转义符：\\，反斜线，将一些有特殊意义的字符（如()、\*、+、?）变为普通字符。
```Python
>>> s = '''1，《凡人修仙传》忘语《诛仙》
2，《红楼梦》曹雪芹《石头记》
3，《海底两万里》(法)凡尔纳《海底三万里》(法)
4，《安徒生童话集》(丹麦)安徒生《安徒生鬼话集》(法)'''
>>> print(s)
1，《凡人修仙传》忘语《诛仙》
2，《红楼梦》曹雪芹《石头记》
3，《海底两万里》(法)凡尔纳《海底三万里》(法)
4，《安徒生童话集》(丹麦)安徒生《安徒生鬼话集》(法)
>>> 
>>> re.findall('《[^》]*》\([^()]*\)', s)
['《海底两万里》(法)', '《海底三万里》(法)', '《安徒生童话集》(丹麦)', '《安徒生鬼话集》(法)']
>>> 
>>> re.findall('《([^》]*)》\(([^()]*)\)', s)
[('海底两万里', '法'), ('海底三万里', '法'), ('安徒生童话集', '丹麦'), ('安徒生鬼话集', '法')]
>>> 
>>> re.findall('《\w+》[(]\w+[)]', s)
['《海底两万里》(法)', '《海底三万里》(法)', '《安徒生童话集》(丹麦)', '《安徒生鬼话集》(法)']
```

python中也有转义"\\"，当二者相遇时有两种方法解决：1，在python的转义项前再加一个反斜线，取消其功能；2，使用 r 原始字符串。

如果正则表达式中也有引号，那么在将其交给python处理（re.findall()）时，要在引号前加反斜线，避免与字符串的起始引号产生冲突。

# 修改替换
re.sub(pattern, repl, s, count=0, flags=0)

修改替换3步走：
1，正确匹配需要被替换的文字；
2，用捕获组标识1的结果中要保留的片段；
3，编写替换后的文本格式，可使用反斜线加数字“\\编号”，代表要保留的捕获组片段。捕获组的编号要从左往右数，无论内外包含。
```Python
>>> s = '''驻巴西大使馆 +55-61-999816188
驻里约热内卢总领馆 +55-21-32376621
驻圣保罗总领馆 +55-11-30610800'''
>>> 
>>> s_new = re.sub(r'\+(\d+)-(\d+)-', '国家号 \\1，地方号 \\2，', s)
>>> print(s_new)
驻巴西大使馆 国家号 55，地方号 61，999816188
驻里约热内卢总领馆 国家号 55，地方号 21，32376621
驻圣保罗总领馆 国家号 55，地方号 11，30610800
>>> 
>>> s_new = re.sub(r'\+(\d+)-(\d+)-', r'国家号 \1，地方号 \2，', s)
>>> print(s_new)
驻巴西大使馆 国家号 55，地方号 61，999816188
驻里约热内卢总领馆 国家号 55，地方号 21，32376621
驻圣保罗总领馆 国家号 55，地方号 11，30610800
>>> 
>>> s_new = re.sub(r'\+(\d+)-(\d+)-', r'国家号 \1，地方号 \2，', s, 1)
>>> print(s_new)
驻巴西大使馆 国家号 55，地方号 61，999816188
驻里约热内卢总领馆 +55-21-32376621
驻圣保罗总领馆 +55-11-30610800
```

 # 反向引用

 匹配重复出现：利用捕获组加反斜线编号
```Python
>>> s = '我是刘刘亦菲，你是刘亦亦菲，他也是刘亦菲菲，我们都是是刘亦菲。'
>>> 
>>> re.findall(r'(\w)\1', s)
['刘', '亦', '菲', '是']
>>> 
>>> re.sub(r'(\w)\1', r'\1', s)
'我是刘亦菲，你是刘亦菲，他也是刘亦菲，我们都是刘亦菲。'
>>> 
>>> s = '''abc,, recover,. aba, abcd, ,. bdcb, , . 。
liuyifei,, liuyifei-l, . lyf《Mulan》2020'''
>>> 
>>> re.sub(r'(\W)[^\n\w]*', r'\1', s)
'abc,recover,aba,abcd,bdcb,\nliuyifei,liuyifei-l,lyf《Mulan》2020'

```
# 案例
```Python
>>> s = '''A公司，发货5000台，联系人：张三，电话 010 -   88223343，邮箱zs@a.com
B公司：联系人：李四，电话：0418-5566778，邮箱：lisi@b.com
C公司发货200台，联系人：王五，电话是（024）66667777，邮箱：wangwu@c.net
D公司电话联系，联系人：赵六，电话0520-33227788，邮箱是 zhaoliu@d.com.
E公司联系人：田七，电话 01083232563，邮箱 tiangqi@163.com
F公司，联系人：John Willams .电话 （001）33243334，邮箱 johnw@mycom.com
G公司，联系人：Ken Wang. 电话（001）56523333，邮箱 778_kenwang@vip.163.com'''

>>> regex = '电话[\s（是：]*\d+[-\s）]*\d+'
>>> re.findall(regex, s)
['电话 010 -   88223343', '电话：0418-5566778', '电话是（024）66667777', '电话0520-33227788', '电话 01083232563', '电话 （001）33243334', '电话（001）56523333']
>>> 
>>> regex = '联系人：[^，.]+'
>>> re.findall(regex, s)
['联系人：张三', '联系人：李四', '联系人：王五', '联系人：赵六', '联系人：田七', '联系人：John Willams ', '联系人：Ken Wang']
>>> 
>>> regex = '联系人：\w+\s*\w+'
>>> re.findall(regex, s)
['联系人：张三', '联系人：李四', '联系人：王五', '联系人：赵六', '联系人：田七', '联系人：John Willams', '联系人：Ken Wang']
>>> 
>>> regex = '电话[\s：是（]*(\d+)[-\s）]*(\d*)'
>>> re.findall(regex, s)
[('010', '88223343'), ('0418', '5566778'), ('024', '66667777'), ('0520', '33227788'), ('01083232563', ''), ('001', '33243334'), ('001', '56523333')]
>>> [''.join(e) for e in re.findall(regex, s)]
['01088223343', '04185566778', '02466667777', '052033227788', '01083232563', '00133243334', '00156523333']
>>> 
>>> for a, b in re.findall('电话[\\s：是（]*(\\d+)[-\\s）]*(\\d*)', s):
        print(a, b)

	
010 88223343
0418 5566778
024 66667777
0520 33227788
01083232563 
001 33243334
001 56523333
>>> 
>>> s = '产品A01的序列号是35-3-401，B35序列号25-7-330-7-2568，产品C09的序列号是:01-48-401-32'
>>> regex = r'([A-Z][0-9]{2})[^\d]+(\d+(?:-\d+)+)'
>>> re.findall(regex, s)
[('A01', '35-3-401'), ('B35', '25-7-330-7-2568'), ('C09', '01-48-401-32')]
>>> 
>>> for i in [e[1] for e in re.findall(regex, s)]:
        re.findall(r'-(\d+)', i)

	
['3', '401']
['7', '330', '7', '2568']
['48', '401', '32']
```