# Web自动化功能测试

总体思路：编写脚本，操纵浏览器，获取页面元素，输入测试数据，获取实际结果，断言预期。

工具：selenium webdriver，浏览器驱动，浏览器 F12，内置库 time.sleep() 。

导包：from selenium import webdriver，from time import sleep

## 第一步：操纵浏览器

```
# 导包
	from selenium import webdriver
	from time import sleep

# 实例化浏览器对象
	driver = webdriver.Chrome()
	driver.maximize_window()
	
# 隐式等待
	driver.implicitly_wait(10)

# 指定地址
	url = "https://www.baidu.com/"
# 本地地址（file:///，r，\ -> /，\ -> \\）
	url = r"file:///D:\yi\index.html"
	url = "file:///D:\\yi\\index.html"
	url = "file:///D:/yi/index.html"
	
# 打开网址
	driver.get(url)

# 获取页面信息
	driver.title
	driver.current_url
	driver.page_source
	
# 关闭浏览器
	driver.quit()
```



## 第二步：操纵页面元素

**定位元素**

​		八种定位：id、name、class_name、tag_name、link_text、partial_link_text、css_selector、xpath

**获取页面元素**

```
# 获取单个元素
	find_element_by_xxx()

# 获取一组元素
	find_elements_by_xxx()
```

**操作页面元素**

```
＃ 获取元素尺寸、文本
	eleobj.size
	eleobj.text

＃ 判断元素是否可见、可用
	eleobj.is_displayed)
	eleobj.is_enabled()

＃ 文本框
	清空：eleobj.clear()
	输入：eleobj.send_keys(str)

＃ 点击
	eleobj.click()

＃ 选择按钮

＃ 选择框

＃ 下拉选择框

# 弹窗
```

**鼠标操作**

```
# 导包
	from selenium.webdriver.common.action_chains import ActionChains

# 鼠标动作执行
	ActionChains(driver).action(eleobj).perform()

# 鼠标动作 action 包括：
	左击：click()
	右击：content_click()
	双击：double_click()
	悬停：move_to_element()
	拖拽：drag_and_drop(source, target)
```

**键盘操作**

```
# 导包
	from selenium.webdriver.common.keys import Keys
	
# 键盘动作执行
	eleobj.send_keys(Keys.action)

# 键盘动作 action 包括：
	删除：Keys.BACK_SPACE
	空格：Keys.SPACE
	制表：Keys.TAB
	回车：Keys.ENTER
	回退：Keys.ESCAPE
	全选：Keys.CONTROL, 'A'
	复制：Keys.CONTROL, 'C'
	剪切：Keys.CONTROL, 'X'
	粘贴：Keys.CONTROL, 'V'
```





## 第三步：断言预期

**获取实际结果**

​		操作参考第二步：操作页面元素

**断言预期**



