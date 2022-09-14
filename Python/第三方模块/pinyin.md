将中文转化为拼音。

pip install pinyin

pinyin.get(s, delimiter='', fomat='diacritical')

```Python
import pinyin

s = '刘亦菲'

# 转换成拼音
>>> pinyin.get(s)
'líuyìfēi'

# 空格分隔
>>> pinyin.get(s, delimiter=' ')
'líu yì fēi'

# 去掉注音声调（format 默认为 diacritical 变音符号）
>>> pinyin.get(s, format='strip')
'liuyifei'

# 空格分隔，将声调以数字的形式显示在拼音后
>>> pinyin.get(s, delimiter=' ', format='numerical')
'liu2 yi4 fei1'

# 获取拼音首字母缩写，默认以空格分隔
>>> pinyin.get_initial(s)
'l y f'
>>> pinyin.get_initial(s, delimiter='')
'lyf'

# 拼音首字母大写，去掉注音，无分隔
>>> ''.join(pinyin.get(s, delimiter=' ', format='strip').title().split())
'LiuYiFei'
```