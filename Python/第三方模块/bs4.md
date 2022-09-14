Beautiful Soup库：解析、遍历、维护“标签树”的功能库。对响应代码进行解析和信息提取

```
导库引用：
	from bs4 import BeautifulSoup
	import bs4
解析信息：
	bs_obj = BeautifulSoup(demo, 'html.parser')
	bs_obj = BeautifulSoup(open(path), 'html.parser')
格式化输出：
	print(bs_obj.prettify())
	tag.prettify()
定位标签：
	直接定位：
		bs_obj.tag：返回第一个指定的标签
		bs_obj('tag')：列表返回所有指定标签
		string='str'：列表返回所有指定字符串str
		bs_obj.find('tag')
		bs_obj.find_all('tag')
	下行遍历：
		.contents：列表返回所有子节点，可遍历
		.children：返回所有子节点，迭代类，可遍历
		.descendants：返回所有子孙节点，生成器类，可遍历
	上行遍历：
		.parent：返回父节点
		.parents：返回所有先辈节点，生成器类，可遍历
			for parent in soup.a.parents:
				if parent is None:
					print(parent)
				else:
					print(parent.name)
	平行遍历：（前提：同一个父节点下）
		.next_sibling：返回后续一个平行节点
		.previous_sibling：返回前续一个平行节点
		.next_siblings：返回后续所有平行节点，生成器类，可遍历
		.previous_siblings：返回前续所有平行节点，生成器类，可遍历
提取信息：
	提取标签名：tag.name
	提取标签内非属性字符串：tag.string、tag.text
	提取标签属性值：
		所有属性键值对，字典形式：tag.attrs
		单个属性键值：tag.attrs['key']
```