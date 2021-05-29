# Web自动化功能测试

总体思路：编写脚本，操纵浏览器，获取页面元素，输入测试数据，获取实际结果，断言预期。

工具：selenium webdriver，浏览器驱动，浏览器 F12，内置库 time.sleep() 。

导包：from selenium import webdriver，from time import sleep

Python **基本操作**

```
# 查看类型
	type(obj)
	
# 查看属性和方法
	dir(obj)
```



## 第一步：浏览器处理

```
# 导包
	from selenium import webdriver
	import time

# 实例化浏览器对象
	driver = webdriver.Chrome()
	
# 获取、设置浏览器尺寸、位置，前进、后退、刷新
	driver.maximize_window()
	driver.minimize_window()
	driver.set_window_size(width, height)
	driver.set_window_position(x, y)
	driver.get_window_size
	driver.get_window_position()
	driver.back()
	driver.forward()
	driver.refresh()
	
# 隐式等待、强制等待，显式等待（暂略）
	driver.implicitly_wait(30)
	time.sleep(1)

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
	driver.quit()	# 关闭所有页面，即关闭浏览器
	driver.close()	# 关闭当前页面，即句柄指向页面
```

**浏览器窗口**

```
# 浏览器每打开一个窗口，就会生存句柄保存于 window_handles 列表中，当前页面是指句柄指向的页面。
# 查看句柄
	driver.current_window_handle
	driver.window_handles

# 切换句柄，切换窗口
	driver.switch_to.window()
```

**窗口截图**

```
# 截图命名
	img_name = time.strftime("%Y%m%d_%H%M%S")
	img_name = str(time.time()).replace('.', '_') 	# 适用于一秒内产生很多图片

# 截图
	driver.get_screenshot_as_file("./images/%s.png" % img_name)
```

## 第二步：页面元素处理

**定位、获取页面元素**

```
# 八种定位
	id、name、class_name、tag_name、link_text、partial_link_text、css_selector、xpath

# 获取单个元素
	find_element_by_xxx()

# 获取一组元素
	find_elements_by_xxx()
```

**操作页面元素**

```
＃ 获取元素尺寸、文本
	ele_obj.size
	ele_obj.text

＃ 判断元素是否可见、可用
	ele_obj.is_displayed)
	ele_obj.is_enabled()

＃ 文本框
	清空：ele_obj.clear()
	输入：ele_obj.send_keys(str)

# 获取元素的属性的值
	ele_obj.get_attribute("attr_name")

＃ 点击
	ele_obj.click()
```

**鼠标操作**

```
# 导包
	from selenium.webdriver.common.action_chains import ActionChains

# 创建对象，执行动作
	ActionChains(driver).action(ele_obj).perform()

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
	ele_obj.send_keys(Keys.action)

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

**Select下拉选择框**

```
# 导包
	from selenium.webdriver.support.select import Select
	
# 定位元素，实例对象
	sel_obj = Select(driver.find_element_by_name("edu"))
	
# 动作    	
	# 选择的三种方式：index、value、visible_text
	sel_obj.select_by_index(1)
	sel_obj.select_by_value("本科")
	sel_obj.select_by_visible_text("初中")
	
	# 遍历选项：sel_obj.options
	for option in sel_obj.options:
    	option.click()
    	time.sleep(1)
    	
	# 通过索引遍历打印选项文本
	for i in range(len(sel_obj.options)):
   		print(sel_obj.options[i].text)

	# 判断是否为多选
	sel_obj.is_multiple
```

**弹框**

```
# 三种弹框：alert()、confirm()、prompt()
	<input type="button" value="alert" onclick="alert('这是警告框！')" />
	<input type="button" value="confirm" onclick="confirm('这是确认框！')" />
	<input type="button" value="prompt" onclick="prompt('这是提示框！', 'liuyifei')" />
	
# 定位弹框按钮，并点击
	driver.find_element_by_xx(str).click()
	
# 切换到弹框
	driver.switch_to.alert
	
# 弹框动作
	alert.text
	alert.send_keys(str)
	alert.accept()
	alert.dismiss()
```

**滚动条**

```
# 执行 js 脚本
	driver.execute_script("window.scrollTo(x, y)")
```

**iframe 内联框架**

```
# html 结构
	<iframe src="https://www.baidu.com" id="ifr_bd" width="40%" height="500px"></iframe>
	<iframe src="https://www.bing.com" id="ifr_bing" width="40%" height="500px"></iframe>
	
# 切换框架
	driver.switch_to.frame(name or id)	# 切换至指定框架，默认取框架的 id 或 name 属性值。
	driver.switch_to.default_content()	# 切换至最外层框架
	driver.switch_to.parent_frame()		# 切换至上一级框架
	
# 注意：内联框架不能直接相互切换，必须先切换至共同的父级框架。关闭窗口 driver.quit() 必须切换至默认的最外层框架。	
```

**验证码处理**

```
# 屏蔽验证码（测试环境采用）

# 设置万能验证码如固定值（生产环境采用）

# 利用验证码识别技术：通过 python-tesseract 来识别验证码，识别率很难达到100%。

# 利用记录 cookie 登录，即绕过账号和验证码登录
	driver.get_cookies()
	driver.add_cookie(dict)
```

## 第三步：断言

**获取实际结果**

​		操作参考第二步：操作页面元素

**断言预期**	

```
# unittest 断言：布尔断言、比较断言、复杂断言
	assert()
	assertEqual()
	...
```

## unittest单元测试框架

总体思路

​		功能的操作步骤封装成函数，写在一个文件夹中；一个功能点写一组测试用例，该组测试用例写在该功能点对应的测试用例类中；执行用例，可执行单个用例、执行全部、按 suite 用例套件自添加用例执行；输出测试报告使用第三方插件 HTMLTestRunner.py。



