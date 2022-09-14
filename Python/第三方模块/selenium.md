python中，可引入 selenium webdriver 来操作浏览器和各种网页元素。操作浏览器，需要先下载与浏览器版本相对应的浏览器驱动程序，并设置路径变量。

本次基于 selenium 3.14

操作 | 指令
---- | ----
安装 | pip show selenium<br>pip install selenium<br>pip install selenium\=\=3.14.0 # 安装指定版本号<br>pip install -U selenium # 安装最新版本号<br>pip uninstall selenium
导包 | from selenium import webdriver
下载浏览器，及对应版本的驱动 | Chrome/Firefox/Edge<br>chromedriver_win32_102.zip

> pip install selenium\=\=3.14.0 # 安装指定版本号
> pip install -U selenium # 安装最新版本号
> pip show selenium # 查看当前包的版本信息
> pip uninstall selenium # 卸载 Selenium

# 浏览器驱动
各浏览器驱动下载地址
GeckoDriver（Firefox）：https://github.com/mozilla/geckodriver/releases
ChromeDriver（Chrome）：https://chromedriver.storage.googleapis.com/index.html
IEDriverServer（IE）：http://selenium-release.storage.googleapis.com/index.html
OperaDriver（Opera）：https://github.com/operasoftware/operachromiumdriver/releases
MicrosoftWebDriver（Edge）：https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver

设置浏览器驱动
我们可以手动创建一个存放浏览器驱动的目录，如D:\\drivers，将下载的浏览器驱动文件（例如 ChromeDriver、GeckoDriver）放到该目录下。
右击“此电脑”，在右键菜单中单击“属性→高级系统设置→高级→环境变量→系统变量→Path”，将“D:\\drivers”目录添加到 Path 中。

验证浏览器驱动
from selenium import webdriver
driver = webdriver.Firefox() # Firefox 浏览器
driver = webdriver.Chrome() # Chrome 浏览器
driver = webdriver.Ie() # Internet Explorer 浏览器
driver = webdriver.Edge() # Edge 浏览器
driver = webdriver.Opera() # Opera 浏览器

# 操作浏览器

操作 | 指令（调用方法或属性）
---- | ----
实例化浏览器对象<br>（打开浏览器） | wd = webdriver.Chrome(r"D:\\driver\\chromedriver.exe")<br>驱动路径已在系统环境变量PATH中：driver = wedriver.Firefox()<br># 关闭chromedriver打印信息<br>options = webdriver.ChromeOptions()<br>options.add_experimental_option('excludeSwitches', \['enable-logging'\])<br>driver = webdriver.Chrome(r"D:\\driver\\chromedriver.exe", options=options)
以手机模式打开浏览器 | mobile_emulation = {"deviceName": "Nexus 5"}<br>chrome_options = webdriver.ChromeOptions()<br>chrome_options.add_experimental_option("mobileEmulation", mobile_emulation)<br>wd = webdriver.Chrome(r"D:\\driver\\chromedriver.exe", desired_capabilities=chrome_options.to_capabilities())
销毁对象，关闭窗口 | 关闭当前窗口：wd.close()<br>退出驱动，并关闭浏览器：wd.quit()
获取浏览器名称 | wd.name
获取并调整窗口尺寸、位置 | wd.minimize_window(), wd.maximize_window()<br>wd.get_window_size(windowHandle)<br>wd.set_window_size(width, height, windowHandle)<br>wd.get_window_position(windowHandle)<br>wd.set_window_position(x, y, windowHandle)<br># 窗口的尺寸、位置<br>wd.get_window_rect()<br>wd.set_window_rect(x,y,width,height)
窗口切换 | wd.switch_to.window(windowHandle)<br>获取当前窗口句柄：wd.current_window_handle<br>获取所有窗口句柄，列表形式：wd.window_handles
打开 url | wd.get(url)<br>本地地址（file:///，r，\ -> /，\ -> \\\）<br>url = r"file:///D:\yi\index.html"<br>url = "file:///D:\\\yi\\\index.html"<br>url = "file:///D:/yi/index.html"
[[#浏览器等待]] | 隐式等待 wd.implicitly_wait(time_to_wait)<br>显式等待<br>强制 import time;time.sleep(seconds)
获取网页信息：url、标题、源代码 | wd.current_url<br>wd.title<br>wd.page_source
网页切换：刷新、前进、后退 | wd.refresh()<br>wd.forward()<br>wd.back()
控制[[#滚动条]]（js） | js = "window.scrollTo(x, y)"<br>wd.execute_script(js)
cookie | wd.get_cookies(), wd.get_cookie(name)<br>wd.add_cookie(cookie_dict)<br>wd.delete_all_cookies(), wd.delete_cookie(name)
截图 | # 保存为 png<br>wd.save_screenshot(filename)<br>wd.get_screenshot_as_file(filename)<br># 保存为二进制 bytes<br>wd.get_screenshot_as_png()<br>with open("xx/xx.png","wb") as f: f.write(wd.get_screenshot_as_png())<br># 命名<br>import time<br>name = time.strftime("%Y%m%d\_%H%M%S")<br>适用于一秒内产生很多图片:name = str(time.time()).replace('.', '\_')<br>wd.get_screenshot_as_file("./images/%s.png" % name)
冻结网页，以便选择页面元素 | 浏览器 F12，Console 输入：setTimeout(function(){debugger}, 5000)

## 浏览器等待
在 UI 自动化测试中，首先要保障测试脚本的稳定运行。但是在实际的测试场景中，由于网络的因素导致需要测试的页面打不开或者打开延迟，从而导致页面元素找不到等各种错误的出现。

在 UI自动化测试中，等待主要存在如下三种形式，分别是：
（1）固定等待。如调用 time模块中的 sleep方法，固定等待几秒。缺点是会延长测试执行的时间。
（2）隐式等待。用到的方法是 implicitly_wait，隐藏式等待指设置最长等待时间。只需设置一 次即可。
（3）显式等待。主要指程序会每隔一段时间执行自定义的程序判断条件，如果判断条件成功，程序就继续执行；如果判断条件失败，程序就会报TimeOutEcpection的异常信息。提供了WebDriverWait类，专门解决网络延迟等待等问题。

### 隐式等待
网页操作时，代码执行的速度比服务器响应的速度快，有的元素内容不是可以立即出现的，可能会等待一段时间。可用隐式等待，即当发现元素没有找到的时候， 并不立即返回找不到元素的错误，而是周期性（每隔半秒钟）重新寻找该元素，直到该元素找到，或者超出指定最大等待时长，这时才抛出异常（find_elements 返回空列表），后续所有的 find_element 或者 find_elements 的方法调用都会采用上面的策略。如果用 time.sleep() 固定等待，等待时间不好设置。
```PYTHON
from time import ctime
from selenium import webdriver
from selenium.common.exceptions import NoSuchElementException

driver = webdriver.Firefox()

# 设置隐式等待为 10s
driver.implicitly_wait(10)
driver.get("http://www.baidu.com")

try:
    print(ctime())
    driver.find_element_by_id("kw22").send_keys('selenium')
except NoSuchElementException as e:
    print(e)
finally:
    print(ctime())

driver.quit()


# 这里同样故意将 id 定位设置为“kw22”，定位失败，执行结果如下。
Sat Feb 16 21:25:21 2019
Message: Unable to locate element: [id="kw22"]
Sat Feb 16 21:25:31 2019
```
implicitly_wait()的参数是时间，单位为秒，本例中设置的等待时间为 10s。首先，这10s 并非一个固定的等待时间，它并不影响脚本的执行速度。其次，它会等待页面上的所有元素。当脚本执行到某个元素定位时，如果元素存在，则继续执行；如果定位不到元素，则它将以轮询的方式不断地判断元素是否存在。假设在第 6s 定位到了元素，则继续执行，若直到超出设置时间（10s）还没有定位到元素，则抛出异常。

### 显式等待
显式等待是 WebDriver 等待某个条件成立则继续执行，否则在达到最大时长时抛出超时异常（TimeoutException）。
```Python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
driver.get("http://www.baidu.com")

# 5s内，每隔0.5s检测until中的条件是否达成，达成则返回条件定位到的元素，没达成继续检测，直到10s，超出时间限制报错。(By.XX, “”)整个作为一个参数。
element = WebDriverWait(driver, 5, 0.5).until(
    EC.visibility_of_element_located((By.ID, "kw"))
    )

element.send_keys('selenium')

driver.quit()
```

WebDriverWait 类是 WebDriver 提供的等待方法。在设置时间内，默认每隔一段时间检测一次当前页面元素是否存在，如果超过设置时间仍检测不到，则抛出异常。具体格式如下。
>WebDriverWait(driver, timeout, poll_frequency=0.5, ignored_exceptions=None)

driver：浏览器驱动。
timeout：最长超时时间，默认以秒为单位。
poll_frequency：检测的间隔（步长）时间，默认为 0.5s。
ignored_exceptions：超时后的异常信息，默认情况下抛出 NoSuchElementException 异常。

WebDriverWait()一般与 until()或 until_not()方法配合使用，下面是 until()和 until_not() 方法的说明。
until()：调用该方法提供的驱动程序作为一个参数，直到返回值为 True。
until_not()：调用该方法提供的驱动程序作为一个参数，直到返回值为 False。

>until(method, message=″)
>until_not(method, message=″)

expected_conditions 类提供的预期条件判断方法如表所示。
调用 expected_conditions 模块中的类之后，一般会返回两种形式的对象实例：一种是布尔类型，返回 True 程序继续执行，如果不是 True，程序就会报 TimeOutExpection 的异常信息；另外一种是返回某个类的具体实例对象，使用该对象就可以直接调用该类中的方法。

方法 | 说明
---- | ----
title_is | 判断当前页面的标题是否等于预期
title_contains | 判断当前页面的标题是否包含预期字符串
presence_of_element_located | 判断元素是否被加在 DOM 树里，并不代表该元素一定可见
visibility_of_element_located | 判断元素是否可见（可见代表元素非隐藏，并且元素的宽和高都不等于 0）
visibility_of | 与上一个方法作用相同，上一个方法的参数为定位，该方法接收的参数为定位后的元素
presence_of_all_elements_located | 判断是否至少有一个元素存在于 DOM 树中。<br>例如，在页面中有 n 个元素的 class 为“wp”，那么只要有一个元素存在于 DOM 树中就返回 True
text_to_be_present_in_element | 判断某个元素中的 text 是否包含预期的字符串
text_to_be_present_in_element_value | 判断某个元素的 value 属性是否包含预期的字符串
frame_to_be_available_and_switch_to_it | 判断该表单是否可以切换进去，如果可以，返回 True 并且切换进去，否则返回 False
invisibility_of_element_located | 判断某个元素是否不在 DOM 树中或不可见
element_to_be_clickable | 判断某个元素是否可见并且是可以点击的
staleness_of | 等到一个元素从 DOM 树中移除
element_to_be_selected | 判断某个元素是否被选中，一般用在下拉列表中
element_selection_state_to_be | 判断某个元素的选中状态是否符合预期
element_located_selection_state_to_be | 与上一个方法作用相同，只是上一个方法参数为定位后的元素，该方法接收的参数为定位
alert_is_present | 判断页面上是否存在 alert

除 expected_conditions 类提供的丰富的预期条件判断方法外，还可以利用前面学过的 is_displayed() 方法自己实现元素显示等待。
```python
from time import sleep, ctime
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("http://www.baidu.com")

print(ctime())
for i in range(10):
    try:
        el = driver.find_element_by_id("kw22")
        if el.is_displayed():
            break
    except:
        pass
    sleep(1)
else:
    print("time out")
print(ctime())
    
driver.quit()


# 这里故意将 id 定位设置为“kw22”，定位失败，执行结果如下。
Sat Feb 16 21:20:37 2019
time out
Sat Feb 16 21:20:48 2019
```
首先 for 循环 10 次，然后通过 is_displayed() 方法循环判断元素是否可见。如果为 True，则说明元素可见，执行 break 跳出循环；否则执行 sleep() 休眠 1s 后继续循环判断。10 次循环结束后，如果没有执行 break，则执行 for 循环对应的 else 语句，打印“time out”信息。

## 执行 js
Selenium 也提供了对 JavaScript 的处理，例如，滑动到浏览器的底部或者顶端，对时间控件及对富文本等的处理，都需要 JavaScript。

WebDriver 提供了 execute_script()和 execute_async_scrip()两种方法来执行 JavaScript 代码，其区别如下：
（1）execute_script 为同步执行且执行时间较短。WebDriver 会等待同步执行的结果，然后执行后续代码。
（2）execute_async_script 为异步执行且执行时间较长。WebDriver 不会等待异步执行代码的结果，而是直接执行后续的代码。

jQuery 操作页面元素
jQuery 是 JavaScript 的一个类库，在 JavaScript 基础上深度封装。与 JavaScript 相比，它可以用更少的代码实现同样的功能。它是元素定位的另一个强有力的补充。
![[jQuery选择器.png]]
```PYTHON
#coding=utf-8
from selenium import webdriver

driver = webdriver.Chrome()
driver.maximize_window()
driver.get("https://www.baidu.com")

jq = "$('#kw').val('selenium')"
driver.execute_script(jq)

#单击“百度一下”按钮
jq = "$('#su').click()"
driver.execute_script(jq)

driver.quit()
```

### 滚动条 
有些页面操作不能依靠 WebDriver 提供的 API 来实现，如浏览器滚动条的拖动。这时就需要借助 JavaScript 脚本。WebDriver 提供了 execute_script()方法来执行 JavaScript 代码。

window.scrollTo()方法用于设置浏览器窗口滚动条的水平位置和垂直位置。第一个参数表示水平的左边距，第二个参数表示垂直的上边距。
```Python
# JS 代码
js = "window.scrollTo(x,y)"    # 水平左边距 x，垂直上边距 y
js = "window.scrollTo(0,document.body.scrollHeight)" # 滑到底部
js = "var q = document.documentElement.scrollTop=n"  # n 为从顶部往下滚动距离

dr.execute_script('window.scrollTo(0,1300)')
dr.execute_script('var q = document.documentElement.scrollTop=0')

for i in range(100):
    dr.execute_script('window.scrollTo(0,%d)' % (i*100))
    time.sleep(1)
```

### 富文本
对于富文本的操作不能按照常规的元素定位方式去定位。富文本一般都在 iframe 框架中，所以需要通过 iframe 的 ID 或者索引的方式进入到 iframe 中，再通过 JaveScript 的方式在富文本中输入需要的内容。例如，要想在微信公众号中编写文章，那么首先需要进入到 iframe。微信公众号富文本部分的 HTML 代码如图 所示。
![[富文本html代码.png]]
从图中可以看到，iframe的 ID为 ueditor_0，那么如果想要在该富文本里面写入内容，可单独写一个可调用函数，参数是编写的内容，代码如下：
```PYTHON
def richText(content):
    ''' 往富文本里面写入内容 '''
    js="document.getElementById('ueditor_0').contentWindow." \
        "document.body.innerHTML='{0}'".format(content)
    driver.execute_script(js)


richText('Python 自动化测试实战')
```

### 时间控件
时间控件在测试中也是经常遇到的，特别是在查询中，需要按某一时间段查询出数据并且来验证查询功能是否正确。但是，大多数时间控件是只读属性，需要手动选择时间控件。在自动化测试中怎样自动输入时间呢？时间控件的 UI 交互如图所示。
![[时间控件UI交互.png]]
这部分的 HTML代码如图所示。
![[时间控件html代码.png]]
要实现在只读属性的时间控件中输入时间，步骤为：
（1） 取消时间控件的只读属性。
（2） 取消只读属性后，在 Input 输入框中给 Value 赋值，填写需要输入的时间。
结合以上步骤，在以上时间控件中，编写开始和结束时间的可调用函数，代码如下：
```PYTHON
#!/usr/bin/env python
#-*-coding:utf-8-*-
#author:wuya

from selenium import webdriver
import time as t

def startJs(startTime):
    ''' 开始时间中输入自定义的时间 '''
    js="$(\"input[placeholder='开始时间≥当前时间']\").removeAttr('readonly');" \
        "$(\"input[placeholder='开始时间≥当前时间']\").attr('value','{0}')".format(startTime)
    driver.execute_script(js)
    
def endJs(endTime):
    ''' 结束时间中输入自定义的时间 '''
    js="$(\"input[placeholder='结束时间>开始时间']\").removeAttr('readonly');" \
        "$(\"input[placeholder='结束时间>开始时间']\").attr('value','{0}')".format(endTime)
    driver.execute_script(js)

driver=webdriver.Firefox()
driver.implicitly_wait(30)
driver.get('file:///C:/Users/Administrator/Desktop/Time/Time/index.html')
startJs('2018-01-01 00:00:00')
t.sleep(3)
endJs('2018-08-08 23:59:59')
driver.quit()
```
# 定位元素

操作 | 指令（调用方法或属性）
---- | ----
定位元素 | el = wd.find_element_by_xx(value)<br># 定位一组元素，找不到时返回空列表<br>els = wd.find_elements_by_xx(value)
使用 By.XX | from selenium.webdriver.common.by import By<br>el = wd.find_element(By.XX, value)<br>els = wd.find_elements(By.XX, value)
定位方式 | id, name, tag_name, class_name, link_text, partial_link_text, css_selector, xpath<br>By.XX：ID, NAME, TAG_NAME, CLASS_NAME, LINK_TEXT, PARTIAL_LINK_TEXT, CSS_SELECTOR, XPATH

find_element 选择符合条件的第一个元素，找不到会抛出 NoSuchElementException 异常。
find_elements 选择符合条件的所有元素，找不到返回空列表。

WebDriver 对象和 WebElement 对象，都可以调用 find_element_by_xxx 和 find_elements_by_xx 方法，WebDriver 对象选择元素的范围是整个 Web 页面，WebElement 对象选择元素的范围是该元素的内部。

## CSS 定位
CSS 使用选择器为页面元素绑定属性。CSS 选择器可以较为灵活地选择控件的任意属性，一般情况下，CSS 定位速度比 XPath 定位速度快。

选择器 | 例子 | 描 述
---- | ---- | ----
.class | .intro | class 选择器，选择 class="intro"的所有元素
\#id | \#firstname | id 选择器，选择 id="firstname"的所有元素
\* | \* | 选择所有元素
element | p | 标签选择，选择所有\<p\>元素
element > element | div > input | 选择父元素为 \<div\>的所有 \<input\> 元素
element + element | div + input | 选择同一级中紧接在 \<div\> 元素之后的所有 \<input\> 元素
\[attribute=value\] | \[target=\_blank\] | 选择 target="\_blank" 的所有元素
, | p,input | 
\[attribute\*=value\] | \[class\*=s_ipt_wr\] | 选择 class 中包含“s_ipt_wr”的所有元素
\[attribute^=value\] | \[class^=bg\] | 选择 class 以“bg”开头的所有元素
\[attribute\$=value\] | \[class\$=wrap\] | 选择 class 以"wrap"结尾的所有元素

```
# 属性选择：id #、class .、attribute [attr[=value]]，多属性值时可写一个。
    # 包含 [attr*=str]
    # 以xx开头 [attr^=str]
    # 以xx结尾 [attr$=str]
    # 标签属性选择 tag[attr=value]
# 层级选择：后代 空格、父子 >、平级 + | ~
    # 后代选择：div p、div *
    # 子代选择：div > p、div > *
        # 父元素的第几个子元素：:nth-child()、tag:nth-child()
        # 父元素的倒数第几个子元素：:nth-last-child()、tag:nth-last-child()
        # 父元素的第几个某类型的子元素：tag:nth-of-type()、tag:nth-last-of-type()
    # 兄弟节点选择
        # 所有后续：tag ~ *、tag1 ~ tag2
        # 相邻后续：tag + tag2
```

find_element_by_css_selector()
```PYTHON
find_element_by_css_selector(".s_ipt")
find_element_by_css_selector(".s_btn")

find_element_by_css_selector("#kw")
find_element_by_css_selector("#su")

find_element_by_css_selector("input")

find_element_by_css_selector("span > input")

find_element_by_css_selector("[autocomplete=off]")
find_element_by_css_selector("[name='kw']")
find_element_by_css_selector('[type="submit"]')

find_element_by_css_selector("form.fm > span > input.s_ipt")
find_element_by_css_selector("form#form > span > input#kw")

find_element_by_css_selector("[class*=s_ipt_wr]")

find_element_by_css_selector("[class^=bg]")

find_element_by_css_selector("[class$=wrap]")

find_element_by_css_selector("form > input:nth-child(2)")
```

## XPath 定位
find_element_by_xpath()

1，绝路路径定位
主要用标签名的层级关系来定位元素的绝对路径，最外层为 html，在 body 文本内，一级一级往下查找。如果一个层级下有多个相同的标签名，那么就按上下顺序确定是第几个。例如，div[2]表示当前层级下第二个 div 标签。
```PYTHON
find_element_by_xpath("/html/body/div/div[2]/div/div/div/from/span/input")
find_element_by_xpath("/html/body/div/div[2]/div/div/div/from/span[2]/input
")
```

2，利用元素属性定位
如果不想指定标签名，那么可以用星号（\*）代替。
```PYTHON
find_element_by_xpath("//input[@id='kw']")
find_element_by_xpath("//input[@id='su']")
find_element_by_xpath("//*[@name='wd']")
find_element_by_xpath("//*[@class='s_ipt']")
find_element_by_xpath("//input[@maxlength='100']")
find_element_by_xpath("//input[@autocomplete='off']")
find_element_by_xpath("//input[@type='submit']")
```

3，层级与属性结合
如果一个元素本身没有可以唯一标识这个元素的属性值，那么我们可以查找其上一级元素。
我们可以通过这种方法一级一级向上查找，直到找到最外层的\<html\>标签，那就是一个绝对路径的写法了。
```PYTHON
find_element_by_xpath("//span[@class='bg s_ipt_wr']/input")
find_element_by_xpath("//form[@id='form']/span/input")
find_element_by_xpath("//form[@id='form']/span[2]/input")
```

4，使用逻辑运算符
如果一个属性不能唯一区分一个元素，那么我们可以使用逻辑运算符连接多个属性来查找元素。
```PYTHON
find_element_by_xpath("//input[@id='kw' and @class='s_ipt']")
```

5，使用 contains 方法
contains 方法用于匹配一个属性中包含的字符串。例如，span 标签的 class 属性为“bg s_ipt_wr”。
```PYTHON
find_element_by_xpath("//span[contains(@calss,'s_ipt_wr')]/input")
```

6，使用 text() 方法
text()方法用于匹配显示文本信息。例如，前面通过 link text 定位的文字链接。
contains 和 text() 配合使用，实现了 partial link 定位的效果。
```PYTHON
find_element_by_xpath("//a[text(),'新闻')]")
find_element_by_xpath("//a[contains(text(),'一个很长的')]")
```

```
# 绝对路径 /、相对路径 //、通配符 *、组选择 |
# 标签选择 //tag
# 属性选择 //tag[@attr]、//tag[@attr='value']，多属性值时要全写上。
    # 包含 [contains(@attr, 'str')]
    # 以xx开头 [starts-with(@attr, 'str')]
    # 以xx结尾 [ends-with(@attr, 'str')]	# xpath 2.0 语法，当前不支持
    # 标签属性选择 //tag[@attr='value']、//*[@attr='value']
# 层级选择
    # 后代选择：//div//*、//div//p
    # 子节点选择 /：//div/*、//div/p
        # 第几个子元素：//div/*[2]、//div/p[2]
        # 倒数第几个子元素：//div/*[last()]、//div/p[last()-1]
        # 范围选择子元素：//tag[position()<=2]、//tag[position()<3]、//tag[position()>=last()-2]
    # 父节点选择：/..
    # 兄弟节点选择：
        # 前续兄弟节点：/preceding-sibling::*、/preceding-sibling::tag
        # 后续兄弟节点：/following-sibling::*、/following-sibling::tag
        
# 使用 WebElement 对象来选择元素时，XPath 路径前需要加'.',表示在当前对象中选择。css selector 中无此问题。eg:
    wd.get("http://cdn1.python3.vip/files/selenium/test1.html")
    web_ele = wd.find_element_by_css_selector('#china')
    for element in web_ele.find_elements_by_xpath(".//p"):
        print(element.get_attribute('outerHTML'))

    for element in web_ele.find_elements_by_css_selector('p'):
        print(element.get_attribute('outerHTML'))
```

# 操作页面元素

操作 | 指令（调用方法或属性）
---- | ----
获取元素的属性值 | el.get_attribute('attr')
获取元素整个HTML标签内容 | el.get_attribute(‘outerHTML')
获取元素内部的HTML标签内容 | el.get_attribute('innerHTML')
获取尺寸 | el.size
获取元素里的文本 | el.text<br>el.get_attribute('textContent')<br>el.get_attribute('innerText')
判断元素可见 | el.is_displayed()
判断元素可用 | el.is_enabled()
点击元素 | el.click()。点击的是该元素的 中心点 位置。
提交表单 | submit()，可模拟搜索框不提供搜索按钮，而是通过回车键完成搜索内容的提交。<br>wd.find_element_by_name('form').submit()
文本框 input | el.send_keys()<br>el.clear()<br>获取输入框里的文本：el_input.get_attribute('value')
上传文件 | 先定位到上传的input，再使用 send_keys(file) 添加文件。<br>el.send_keys(file)
单选框 radio | el.click()
复选框 checkbox | el.click()，先去掉默认勾选项，再勾选。
判断是否选中 | el.is_selected()
下拉选择框 select | 先定位到 select 元素，再实例化 Select(sel_el)，再调用方法<br>from selenium.webdriver.support.select import Select<br>sel_dept = Select(wd.find_element('name', 'dept'))<br>sel_dept.select_by_index(3)<br>sel_dept.select_by_value('人事部')<br>sel_dept.select_by_visible_text('项目部')<br>sel_dept.options<br>sel_dept.is_multiple<br>sel_dept.deselect_all()<br>sel_dept.all_selected_options
三种浏览器弹框<br>alert/confirm/prompt | wd.find_element(by, value).click()<br>al = wd.switch_to.alert<br>al.accept(),al.dismissed, al.send_keys(), al.text
鼠标 | 导入动作链类：from selenium.webdriver import ActionChains<br>执行动作：ActionChains(wd).action(el).perform()<br>action：悬停	move_to_element()、右击	context_click()、双击 double_click()、拖动 drag_and_drop()
键盘 | 导入keys类：from selenium.webdriver.common.keys import Keys<br>键盘输入：el.send_keys(Keys.xxx)<br>按键：CONTROL, 'a'、CONTROL, 'c'、CONTROL, 'x'、CONTROL, 'v'、F1、F12、BACK_SPACE、SPACE、TAB、ENTER、ESCAPE

## 单选、复选、下拉
```python
# radio，单选按钮：选择元素，点击
el.click()
	
# checkbox，复选框：先选择所有默认勾选元素，依次点击所有去掉默认勾选，再点击选择元素
el.click()
	
# Select 单选 多选
# 导包
>>> from selenium.webdriver.support.select import Select
	
# 定位元素，实例对象
>>> sel_dept = Select(wd.find_element('name', 'dept'))
	
# 动作    	
# 选择的三种方式：index、value、visible_text
>>> sel_dept.select_by_index(3)
>>> sel_dept.select_by_value('人事部')
>>> sel_dept.select_by_visible_text('项目部')
	
# 遍历选项：sel_dept.options
for option in sel_dept.options:
    option.click()
    
# 通过索引遍历打印选项文本
for i in range(len(sel_dept.options)):
    print(sel_dept.options[i].text)

# 判断是否为多选
sel_dept.is_multiple

# 去掉默认选择
if sel_dept.is_multiple:
    sel_dept.deselect_all()
	

>>> wd.get(r"file:///D:/workspace/www/learnweb/index.html")

# radio
>>> els_gender = wd.find_elements('name', 'gender')
>>> els_gender[-1].click()

# checkbox
>>> for e in wd.find_elements('css selector', 'input[checked="checked"]'):
	e.click()
>>> 
>>> for hobby in ['游戏', 'girl','美术']:
	wd.find_element('css selector', 'input[name="hobby"][value=%s]' % hobby).click()
	
# Select multiple
>>> sel_dept = Select(wd.find_element('name', 'dept'))
>>> 
>>> if sel_dept.is_multiple:
	sel_dept.deselect_all()
>>> 
>>> for e in sel_dept.options:
	e.text, e.get_attribute('value')

	
('人事部', '人事部')
('财务部', '财务部')
('市场部', '市场部')
('研发部', '研发部')
('后勤部', '后勤部')
('保卫部', '保卫部')
('项目部', '项目部')
>>> 
>>> sel_dept.select_by_index(3)
>>> sel_dept.select_by_value('人事部')
>>> sel_dept.select_by_visible_text('项目部')
>>> 
>>> for e in sel_dept.all_selected_options:
	e.text

	
'人事部'
'研发部'
'项目部'
>>> sel_dept.deselect_all()
>>> for text in ["财务部", "项目部", "研发部"]:
	sel_dept.select_by_visible_text(text)

	
>>> for e in sel_dept.all_selected_options:
	e.text

	
'财务部'
'研发部'
'项目部'
```

## 警告、确认、提示框
```HTML
# 三种浏览器弹框：alert()、confirm()、prompt()
<input type="button" value="alert" onclick="alert('这是警告框！')" />
<input type="button" value="confirm" onclick="confirm('这是确认框！')" />
<input type="button" value="prompt" onclick="prompt('这是提示框！', 'liuyifei')" />
```

弹框操作
```PYTHON
# 定位弹框按钮，并点击
wd.find_element_by_xx(str).click()
	
# 切换到弹框
wd.switch_to.alert
	
# 弹框动作
alert.text
alert.send_keys()
alert.accept()
alert.dismiss()
```

## iframe 内联框架
Frame 标签有 Frameset、Frame 和 iFrame 三种。Frameset 可以直接按照正常元素定位。Frame 和 iFrame 的定位方法相同，需要把驱动切换到 Frame 内再进行操作。

在html语法中，frame 元素 或者iframe元素的内部 会包含一个 被嵌入的 另一份html文档。
在我们使用selenium打开一个网页是， 我们的操作范围 缺省是当前的 html ， 并不包含被嵌入的html文档里面的内容。如果我们要操作被嵌入的 html 文档 中的元素， 就必须切换操作范围到被嵌入的文档中。
```HTML
<iframe src="https://www.baidu.com" id="ifr_bd" width="40%" height="500px"></iframe>
<iframe src="https://www.bing.com" id="ifr_bing" width="40%" height="500px"></iframe>
```

切换框架
switch_to.frame() 默认可以直接对框架的 id 属性或 name 属性传参，因而可以定位元素的对象。
注意：内联框架不能直接相互切换，必须先切换至共同的父级框架。关闭窗口 wd.quit() 必须切换至默认的最外层框架。
```PYTHON
# 根据 id、name、index、元素对象 这四种方式来切换到指定框架
wd.switch_to.frame(id)
wd.switch_to.frame(name)
wd.switch_to.frame(index)
wd.switch_to.frame(WebElement)

wd.switch_to.default_content()		# 切换至最外层框架
wd.switch_to.parent_frame()		# 切换至上一级框架
```

## 鼠标事件
```python
# 导包
from selenium.webdriver.common.action_chains import ActionChains

# 创建对象，执行动作
ActionChains(wd).action(el).perform()

# 鼠标 action 见下表。

>>> ActionChains(wd).click(wd.find_element('css selector', 'figure a')).perform()
```

鼠标 action | 说明
---- | ----
click(on_element=None) | 鼠标单击操作。
click_and_hold(on_element=None) | 鼠标单击并且按住不放。
double_click(on_element=None) | 鼠标双击。
context_click(on_element=None) | 鼠标右击操作。
drag_and_drop(source,target) | 鼠标拖曳。
drag_and_drop_by_offset(source,xoffset,yoffset) | 将目标拖曳到目标位置。
key_down(value,element=None) | 按住某个键，实现快捷键操作。
key_up(value,element=None) | 松开某个键，一般和 key_down 操作一起使用。
move_to_element(to_element) | 将鼠标移到指定的某个页面元素，悬停。
move_to_element_with_offset(to_element,xoffset,yoffset) | 移动鼠标至指定的坐标。
release(on_element=None) | 释放按下的鼠标。
move_by_offset(x, y) | 移动鼠标到指定坐标。
reset_action() | 重置 action。
perform() | 将之前一系列的 ActionChains 执行。

## 键盘事件

```python
# 导包
from selenium.webdriver.common.keys import Keys
	
# 键盘动作执行
el.send_keys(Keys.action)

# 键盘 action 见下表。
```

键盘 action | 说明
---- | ----
Keys.BACK_SPACE | 删除
Keys.SPACE | 空格
Keys.TAB | 制表
Keys.ENTER | 回车
Keys.ESCAPE | 回退
Keys.CONTROL, 'A' | 全选
Keys.CONTROL, 'C' | 复制
Keys.CONTROL, 'X' | 剪切
Keys.CONTROL, 'V' | 粘贴
Keys.F1 | F1
Keys.F12 | F12

## 验证码
验证码处理思路：
1，利用记录 cookie 登录，即绕过账号和验证码登录：driver.get_cookies(), driver.add_cookie()
2，用图像识别技术来处理验证码，将图片上的字符进行识别并转化成文本字符串。
3，屏蔽验证码（测试环境采用）
4，设置万能验证码，如固定值（生产环境采用）

例：使用 Cookie 绕过百度网盘验证登录
1，通过脚本抓取初次打开百度网盘首页的 Cookie（假如之前登录过，建议先清理一下 Cookie）
2，手动登录百度网盘，再抓取一次 Cookie（脚本如下所示）。跟上一步的脚本相比，这里增加了一个在执行抓取 Cookie 操作之前的长时间等待。在等待的过程中，测试人员
将手动登录百度网盘；
3，比较一下两次得到的 Cookie 值的差别，找出哪些项是在第一次 Cookie 值中没有的。通过添加这些缺少的值，来实现自动化登录：打开网址，添加cookie（循环遍历上一步获取的cookies添加），再打开网址，看是否已登录。
```PYTHON
>>> from selenium import webdriver
>>> import time
>>> 
>>> driver = webdriver.Chrome()
>>> driver.get("https://pan.baidu.com/")
>>> cookies = driver.get_cookies()
>>> print(cookies)
[{'domain': '.baidu.com', 'expiry': 1694506631, 'httpOnly': False, 'name': 'BAIDUID', 'path': '/', 'secure': False, 'value': '1BE12D26DC1665E2D915A7D4765AFB45:FG=1'}, {'domain': '.baidu.com', 'expiry': 1694506631, 'httpOnly': False, 'name': 'BAIDUID_BFESS', 'path': '/', 'sameSite': 'None', 'secure': True, 'value': '1BE12D26DC1665E2D915A7D4765AFB45:FG=1'}, {'domain': '.baidu.com', 'expiry': 1665562631, 'httpOnly': True, 'name': 'newlogin', 'path': '/', 'secure': False, 'value': '1'}, {'domain': 'pan.baidu.com', 'httpOnly': False, 'name': 'csrfToken', 'path': '/', 'secure': False, 'value': '5NOUOQWg-V6TADGDwGjY_JSn'}]
>>> driver.maximize_window()
>>> 
# 手动登陆百度网盘后，记录cookies
>>> time.sleep(30)
>>> cookies2 = driver.get_cookies()
>>> print(cookies2)
[{'domain': '.baidu.com', 'expiry': 1662977993, 'httpOnly': True, 'name': 'ab_sr', 'path': '/', 'sameSite': 'None', 'secure': True, 'value': '1.0.1_NzMwNTBmZjkyYWNhZThhOGU1YTIxODljMzExNTEyNzk2OGQxYjdiMDkyZWUyY2M3MjcyMjg4MWM0MTJiOGFlYmNmMDhlZWZlYTFiMmE1OGM4MTI0OTdiYTBjOTExMTg0OGI5NjBlNmMyOGM4ZjBjNTE4NjUyYmJjM2RiYWM0NzBjNWYzMTEzNDM3MzY0YTVhYjhlYWJhMTg5NWIxOGJlOA=='}, {'domain': 'pan.baidu.com', 'expiry': 1665562793, 'httpOnly': False, 'name': 'ndut_fmt', 'path': '/', 'secure': False, 'value': '25CC22C226FBD4C9C1A533F1F8AA8E997333CFF0D4E4183E8AEB15A59635B5F1'}, {'domain': '.pan.baidu.com', 'expiry': 1665649187, 'httpOnly': True, 'name': 'STOKEN', 'path': '/', 'secure': False, 'value': '9782cba242567fcc6a81dd1ff8dc585450a4477cd01f1bc5d920e68ee7ee4811'}, {'domain': 'pan.baidu.com', 'httpOnly': False, 'name': 'csrfToken', 'path': '/', 'secure': False, 'value': '5NOUOQWg-V6TADGDwGjY_JSn'}, {'domain': '.baidu.com', 'expiry': 1697530786, 'httpOnly': True, 'name': 'BDUSS_BFESS', 'path': '/', 'sameSite': 'None', 'secure': True, 'value': 'XVPMVN2VUtFYS1EN2ZzWG1uS0s5QkU2TkJ3ajlUdnB0N003TzFESGVSdWJlRVpqRVFBQUFBJCQAAAAAAAAAAAEAAABJlfqEeWl4aWFuanVuODA1MjEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAJvrHmOb6x5je'}, {'domain': '.pan.baidu.com', 'expiry': 1663057191, 'httpOnly': True, 'name': 'PANPSC', 'path': '/', 'secure': False, 'value': '12619586030129052077%3ACU2JWesajwCeXXvOfQx0BMJKt428qvGuWoxRQG2rjNlPfISYI4bxdCvbCphtI0nMm3Ru%2FgeZHMD%2Ftj4%2FRzsO%2B5%2FkRuWDh5J%2FdDUoQB%2Bj8B8d%2Bd6J43yXeRBQd5JJtSGFSzMUJ31pDizEW4EhbfLXnpC9qgHiau%2FldS8c7ndIzLExGZiuUUeCaoX0p6z9lD8V7g%2B5PM2vWus%3D'}, {'domain': '.baidu.com', 'expiry': 1697530786, 'httpOnly': True, 'name': 'BDUSS', 'path': '/', 'secure': False, 'value': 'XVPMVN2VUtFYS1EN2ZzWG1uS0s5QkU2TkJ3ajlUdnB0N003TzFESGVSdWJlRVpqRVFBQUFBJCQAAAAAAAAAAAEAAABJlfqEeWl4aWFuanVuODA1MjEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAJvrHmOb6x5je'}, {'domain': '.baidu.com', 'expiry': 1694506631, 'httpOnly': False, 'name': 'BAIDUID_BFESS', 'path': '/', 'sameSite': 'None', 'secure': True, 'value': '1BE12D26DC1665E2D915A7D4765AFB45:FG=1'}, {'domain': 'pan.baidu.com', 'httpOnly': False, 'name': 'XFT', 'path': '/disk', 'secure': False, 'value': 'mfB8ECn3MB4dxna6bS2/Mfy9TLFiDNJToulRgKQG8lE='}, {'domain': '.baidu.com', 'expiry': 1694506631, 'httpOnly': False, 'name': 'BAIDUID', 'path': '/', 'secure': False, 'value': '1BE12D26DC1665E2D915A7D4765AFB45:FG=1'}, {'domain': '.baidu.com', 'expiry': 1665562631, 'httpOnly': True, 'name': 'newlogin', 'path': '/', 'secure': False, 'value': '1'}, {'domain': 'pan.baidu.com', 'httpOnly': False, 'name': 'XFCS', 'path': '/disk', 'secure': False, 'value': '15FC73014B781963BC91E555D4AC7669E2C5E336B73966E33F2104A689077ABA'}, {'domain': 'pan.baidu.com', 'httpOnly': False, 'name': 'XFI', 'path': '/disk', 'secure': False, 'value': '82f8160d-3e59-fac3-81b9-ea7a80c6867e'}]
>>> 
>>> driver.get("https://pan.baidu.com/")
>>> cookies = driver.get_cookies()
>>> for coo in cookies2:
    	driver.add_cookie(coo)

	
>>> time.sleep(5)
>>> driver.get("https://pan.baidu.com/")
>>> driver.quit()
```
## 上传文件
上传文件是比较常见的 Web 功能之一，但 WebDriver 并没有提供专门用于上传的方法，实现文件上传的关键在于思路。

在 Web 页面中，文件上传操作一般需要单击“上传”按钮后打开本地 Windows 窗口，从窗口中选择本地文件进行上传。

在 Web 页面中一般通过以下两种方式实现文件上传。
普通上传：将本地文件路径作为一个值放在 input 标签中，通过 form 表单将这个值提交给服务器。
插件上传：一般是指基于 Flash、JavaScript 或 Ajax 等技术实现的上传功能。

如果是 input 类型的标签，并且 type 值为“file”，可以将其看作一个输入框，那么可以直接通过 send_keys 的方式来绕过弹出框操作，直接将文件信息传递给“添加附件”按钮。
```PYTHON
#其中“filepath”请填写真实的文件路径
driver.find_element_by_name("UploadFile").send_keys("filepath")
```

对于插件上传，可以使用 AutoIt 来实现。

## 下载文件
WebDriver 允许我们设置默认的文件下载路径，也就是说，文件会自动下载并且存放到设置的目录中，不同的浏览器设置方式不同。

以 Chrome 浏览器为例，演示文件下载。

```PYTHON
import os
from selenium import webdriver

options = webdriver.ChromeOptions()
prefs = {'profile.default_content_settings.popups': 0,
        'download.default_directory': os.getcwd()}
options.add_experimental_option('prefs', prefs)

driver = webdriver.Chrome(chrome_options=options)
driver.get("https://pypi.org/project/selenium/#files")
driver.find_element_by_partial_link_text("selenium-3.141.0.tar.gz").click()
```

Chrome 浏览器在下载时默认不会弹出下载窗口，这里主要想修改默认的下载路径。

>profile.default_content_settings.popups

设置为 0，表示禁止弹出下载窗口。

>download.default_directory

设置文件下载路径，使用os.getcwd()方法获取当前脚本的目录作为下载文件的保存位置。

以 Firefox 浏览器为例，演示文件下载。

```PYTHON
import os
from selenium import webdriver

fp = webdriver.FirefoxProfile()
fp.set_preference("browser.download.folderList", 2)
fp.set_preference("browser.download.dir", os.getcwd())
fp.set_preference("browser.helperApps.neverAsk.saveToDisk", "binary/octet-stream")

driver = webdriver.Firefox(firefox_profile=fp)
driver.get("https://pypi.org/project/selenium/#files")
driver.find_element_by_partial_link_text("selenium-3.141.0.tar.gz").click()
```

为了能在 Firefox 浏览器中实现文件的下载，我们需要通过 FirefoxProfile()对其做一些设置。

>browser.download.folderList

设置为 0，表示文件会下载到浏览器默认的下载路径；设置为 2，表示文件会下载到指定目录。

>browser.download.dir

用于指定下载文件的目录。通过 os.getcwd()方法获取当前文件的所在位置，即下载文件保存的目录。

>browser.helperApps.neverAsk.saveToDisk

指定要下载文件的类型，即 Content-type 值，“binary/octet-stream”用于表示二进制文件。
HTTP Content-type 常用对照表参见  http://tool.oschina.net/commons 。

## textarea
```HTML
<textarea id="id" style="width: 98%" cols="50" rows="5" class="textarea"></textarea>
```

若不能使用 send_keys()，可考虑使用 js。
```PYTHON
text = "input text"
js = "document.getElementById('id').value='" + text + "';"
driver.execute_script(js)
```

首先，定义要输入的内容 text。然后，将 text 与 JavaScript 代码通过“+”进行拼接，这样做的目的是为了方便自定义输入内容。最后，通过 execute_script()执行 JavaScript 代码。

## HTML5 视频
WebDriver 支持在指定的浏览器上测试 HTML5，另外，还可以使用 JavaScript 测试这些功能，这样就可以在任意浏览器上测试 HTML5 了。

大多数浏览器使用插件（如 Flash）播放视频，但是，不同的浏览器需要使用不同的插件。HTML5 定义了一个新的元素\<video\>，指定了一个标准的方式嵌入电影片段。HTML5 Video Player ，IE9+、Firefox、Opera、Chrome 都支持元素\<video\>。

```PYTHON
from time import sleep
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("http://videojs.com/")
video = driver.find_element_by_id("preview-player_html5_api")

# 返回播放文件地址
url = driver.execute_script("return arguments[0].currentSrc;", video)
print(url)

# 播放视频
print("start")
driver.execute_script("arguments[0].play()", video)

# 播放 15s
sleep(15)

# 暂停视频
print("stop")
driver.execute_script("arguments[0].pause()", video)

driver.quit()
```

JavaScript 有个内置的对象叫作 arguments。arguments 包含了函数调用的参数数组，\[0\] 表示取对象的第 1 个值。
currentSrc 返回当前音频/视频的 URL。如果未设置音频/视频，则返回空字符串。
load()、play()和 pause() 控制视频的加载、播放和暂停。

## 滑动解锁
滑动解锁是目前比较流行的解锁方式，当我们单击滑块时，改变的只是 CSS 样式，HTML 代码段如下。
```HTML
<div class="slide-to-unlock-progress" style="background-color: rgb(255, 233, 127); height: 36px; width: 0px;"></div>

<div class="slide-to-unlock-handle" style="background-color: rgb(255, 255, 255); height: 38px; line-height: 38px; width: 37px; left: 0px;"></div>
```
slide-to-unlock-handle 表示滑块。在滑动过程中，滑块的左边距会逐渐变大，因为它在向右移动。
slide-to-unlock-progress 表示滑过之后的背景色，背景色的区域会逐渐增加，因为滑块在向右移动。

```PYTHON
from time import sleep
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.common.exceptions import UnexpectedAlertPresentException

driver = webdriver.Chrome()
driver.get("https://www.helloweba.com/demo/2017/unlock/")

# 定位滑动块
slider = driver.find_elements_by_class_name("slide-to-unlock-handle")[0]
action = ActionChains(driver)
action.click_and_hold(slider).perform()

for index in range(200):
    try:
        action.move_by_offset(2, 0).perform()
    except UnexpectedAlertPresentException:
        break
    action.reset_actions()
    sleep(0.1) # 等待停顿时间
    
# 打印警告框提示
success_text = driver.switch_to.alert.text
print(success_text)
```

click_and_hold()： 单击并按下鼠标左键。
move_by_offset()：移动鼠标，第一个参数为 x 坐标距离，第二个参数为 y 坐标距离。
reset_action()：重置 action。

再看另外一种应用，上下滑动选择日期。参考前面的操作，通过 ActionChains 类可以实现上下滑动选择日期，但是这里要介绍另外一种方法，即通过 TouchActions 类实现上下滑动选择日期。
```PYTHON
from time import sleep
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("http://www.jq22.com/yanshi4976")
sleep(2)
driver.switch_to.frame("iframe")
driver.find_element_by_id("appDate").click()

# 定位要滑动的年、月、日
dwwos = driver.find_elements_by_class_name("dwwo")
year = dwwos[0]
month = dwwos[1]
day = dwwos[2]

action = webdriver.TouchActions(driver)
action.scroll_from_element(year, 0, 5).perform()
action.scroll_from_element(month, 0, 30).perform()
action.scroll_from_element(day, 0, 30).perform()
# ……
```

这里使用 TouchActions 类中的 scroll_from_element()方法滑动元素，参数如下。
on_element：滑动的元素。
xoffset：x 坐标距离。
yoffset：y 坐标距离。

滑块处理思路
Selenium 中对滑块的操作基本是采用元素拖曳的方式，而这种方式需要用到 Selenium 的 Actionchains 功能模块。
先分别求出滑块按钮和滑块区域的长度和宽度。
```PYTHON
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
import time

driver = webdriver.Chrome()
driver.get("https://passport.ctrip.com/user/reg/home")

driver.find_element_by_css_selector("#agr_pop>div.pop_footer>a.reg_btn.reg_agree").click()
time.sleep(3)

#以下代码的功能是获取滑块元素
sour = driver.find_element_by_css_selector("#slideCode>div.cpt-drop-box>div.cpt-drop-btn")
print( sour.size['width'])
print(sour.size["height"])

#以下代码的功能是获取滑块区域元素
ele = driver.find_element_by_css_selector("#slideCode>div.cpt-drop-box>div.cpt-bg-bar")
print( ele.size['width'])
print(ele.size["height"])

#拖动滑块
ActionChains(driver).drag_and_drop_by_offset(sour,ele.size['width'],-sour.size["height"]).perform()
```

# 常见异常
Selenium 中常见的异常有9 种（如表9.1 所示），其中比较常用的是“NoSuchElementException”。

异常方法 | 异常描述
---- | ----
NoSuchElementException | 当选择器返回元素失败时，抛出异常
ElementNotVisibleException | 当要定位的元素在 DOM 中存在，而在页面上不显示、不能交互时，抛出异常
ElementNotSelectableException | 当尝试选择不可选的元素时，抛出异常
NoSuchFrameException | 当要切换到目标 Frame 而 Frame 不存在时，抛出异常
NoSuchWindowException | 当要切换到目标窗口，而该窗口不存在时，抛出异常
Timeout Exception | 当代码执行时间超出时，抛出异常
NoSuchAttributeException | 当元素的属性找不到时，抛出异常
UnexpectedTagNameException | 当支持类没有获得预期的 Web 元素时，抛出异常
NoAlertPresentException | 当一个意外的警告出现时，抛出异常
ElementNotInteractableException | 元素不可互动