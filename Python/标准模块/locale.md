---
tags: locale
---

标准模块 locale，用于设置操作系统的地区、语言、文字格式等信息。

设置本地环境为中文： ^fc29cc
```Python
>>> import locale
>>> locale.setlocale(locale.LC_CTYPE, 'chinese')
'Chinese_China.936'

```