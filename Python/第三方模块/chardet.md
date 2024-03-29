---
tags: 字符编码检测 打开文件指定编码
---

chardet，Character Detect，字符检测的第三方模块。用于检测文件、网页、数据库的编码。

检测原理：根据文本的字节流的数据特征来判断属于哪 一种编码（不同编码有不同的数据特征）。检测的结果并不完全准确的，confidence 置信度。

检测步骤：1 先读取文本的字节流，2 再调用 chardet.detect(byte_str) 识别编码。
```Python
>>> import chardet
>>> 
>>> file = "C:/Users/yi/Desktop/新建文本文档.txt"
>>> with open(file, 'rb') as f:
    	s = f.read()


>>> s
b'\xe9\x99\x95\xe5\x8c\x97\xe6\xb0\x91\xe6\xad\x8c\r\n\t\xe4\xb8\x81\xe6\x96\x87\xe5\x86\x9b - \xe5\xb0\x8f\xe6\xa1\x83\xe7\xba\xa2\r\n\t\xe5\x88\x98\xe5\xa6\x8d - \xe8\x84\xb8\xe8\x9b\x8b\xe8\x9b\x8b\xe7\x83\xab\xe4\xba\x86\xe5\x93\xa5\xe5\x93\xa5\xe7\x9a\x84\xe6\x89\x8b\xe3\x80\x81\xe4\xb8\xba\xe7\x94\x9a\xe4\xb8\x8d\xe5\x9b\x9e\xe5\xae\xb6\r\n\xe5\xb1\xb1\xe8\xa5\xbf\xe6\xb0\x91\xe6\xad\x8c\r\n\t\xe5\x9c\xaa\xe6\xa2\x81\xe6\xa2\x81\r\n\xe7\xa5\x9e\xe4\xbb\x99\xe6\x8c\xa1\xe4\xb8\x8d\xe4\xbd\x8f\xe4\xba\xba\xe6\x83\xb3\xe4\xba\xba\r\ntest'
>>> 
>>> type(s)
<class 'bytes'>
>>> 
>>> chardet.detect(s)
{'encoding': 'GB2312', 'confidence': 0.99, 'language': 'Chinese'}
```