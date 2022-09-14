---
tags: json
---

JSON (JavaScript Object Notation)，JavaScript对象表示法，是一个受 JavaScript 的对象字面量语法启发的轻量级数据交换格式。是存储和交换文本信息的语法，类似 XML。

Python3 中可以使用 json 模块来对 JSON 数据进行编解码。

| json 方法 | 说明 |
| --- | --- |
| json.dumps(obj)<br>json.dump(obj, fp) | 将python数据转为json字符串，强制键转为字符串。 |
| json.loads(json_str)<br>json.load(fp) | 将json字符串转为python数据。 |

```Python
>>> import json
>>> 
>>> my_dict = {"name": "刘亦菲", "age": 18, "gender": "女"}
>>> 
>>> json_str = json.dumps(my_dict)
>>> json_str
'{"name": "\\u5218\\u4ea6\\u83f2", "age": 18, "gender": "\\u5973"}'

>>> temp_d = json.loads(json_str)
>>> temp_d
{'name': '刘亦菲', 'age': 18, 'gender': '女'}

>>> with open(r"C:\Users\yi\Desktop\test.json", 'w', encoding="utf8") as f:
	# f.write(json.dumps(my_dict)
	json.dump(my_dict, f)

	
>>> with open(r"C:\Users\yi\Desktop\test.json", 'r', encoding="utf8") as f:
	cont = json.load(f)

	
>>> cont
{'name': '刘亦菲', 'age': 18, 'gender': '女'}
```