Requests库：向服务器发送请求，获得响应信息代码
	安装：pip install requests
	导库：import requests
	发送请求方法：request()、get()、head()、post()、put()、patch()、delete()
		后六个方法分别对应HTTP协议对资源的操作方法(get/head,post/put/patch,delete)
		r = requests.get(url)
	响应处理：r.status_code、r.text、r.encoding、r.apparent_encoding、r.content、r.url、r.raise_for_status()
		爬取图片、视频：with open(filepath, "wb") as f:f.write(r.content)
		demo =  r.text

```python
# 构造接口地址和请求参数  
url = "http://apis.juhe.cn/simpleWeather/query"  
paras = {"city": "北京", "key": "d6e7aae6149e367cfcc6ad8e04e945d5"}  
  
# 发送 get/post 请求  
r = requests.get(url, params=paras)  
r = requests.post(url, data=paras)  
  
# 获取响应数据  
res = r.json()  
print(res)  
print(res["error_code"])  
print(res["reason"])  
print(res["result"])
```

爬取网页的通用代码框架

```Python
def getHTMLText(url):
	try:
		r = requests.get(url)
		r.raise_for_status()
		r.encoding = r.apparent_encoding
		return r.text
	except:
		return "产生异常"

if __name__ == "__main__":
	url = "http://www.baidu.com"
	print(getHTMLText(url))
```