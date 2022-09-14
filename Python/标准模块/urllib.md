urllib.request

res = request.urlopen()
无论什么文件，其收到的都是一串二进制数字，即字节流。按照文本编码解读，就用作字符串；按照图片格式解读，就显示为图片。

c = res.read()
读取响应的字节流到变量中。

```Python
from urllib.request import urlopen
from bs4 import BeautifulSoup

content = urlopen('https://www.boc.cn/sourcedb/whpj/').read()

# s = content.decode('utf-8')
# print(s)

bs_obj = BeautifulSoup(content, features='lxml')
all_tr = bs_obj.find_all('table')[1].find_all('tr')
for tr in all_tr[1:]:
	all_td = tr.find_all('td')
	print(f'{all_td[0].text} 卖出价：{all_td[4].text}')
```
