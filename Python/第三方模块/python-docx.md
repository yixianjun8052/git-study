---
tags: word读写
---

python-docx，用于读写Word的第三方模块。

```
pip install python-docx

import docx
```

| 方法 | 说明 |
| ---- | ---- |
| doc = docx.Document(fpath) | 打开docx文件，返回Document对象。 |
| content = doc.paragrahps | 所有段落列表。 |
| content\[index\].text | 每一个段落的文本。赋值可用来修改段落文本。 |
| doc.save(fpath) | 保存文件。 |

```Python
>>> doc = docx.Document(r"C:\Users\yi\Desktop\test.docx")
>>> type(doc)
<class 'docx.document.Document'>
>>> 
>>> content = doc.paragraphs
>>> type(content)
<class 'list'>
>>> 
>>> type(content[0])
<class 'docx.text.paragraph.Paragraph'>
>>> content[0].text
'诗文表达笔记'
>>> content[-1].text
'——《读库1700》p251'
>>> 
>>> doc.paragraphs[0].text
'诗文表达笔记'
>>> 
>>> doc.paragraphs[0].text += '-作者 刘亦菲 '
>>> 
>>> doc.save(r"C:\Users\yi\Desktop\test.docx")
>>> 
>>> for p in content:
        p.text += '-作者：刘亦菲-'

	
>>> doc.save(r"C:\Users\yi\Desktop\test.docx")
>>> 
>>> for p in content:
        p.text = ''.join(re.sub('-作者：刘亦菲-', '', p.text))

	
>>> doc.save(r"C:\Users\yi\Desktop\test.docx")
```