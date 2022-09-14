pytest 是一个第三方单元测试框架，更加简单、灵活，而且提供了更加丰富的扩展，弥补了 unittest 在做 Web 自动化测试时的一些不足。

官方网站：https://docs.pytest.org/en/latest/。

```python
pip install pytest

# hmtl 测试报告模块
pip install pytest-html
```

## 基本原理
- 框架结构
	- cases
		- test_case.py
	- lib
		- commonlib.py
	- log
		- report.html

- 寻找测试用例的规则
	- 如果未指定命令行参数如 pytest，则从 testpath（如果已配置）或当前目录开始收集；
	- 如果参数指定了目录如 pytest cases，则按参数来找；
    - 寻找过程会递归到目录中，除非它们匹配上 norecursedirs；
	- 在这些目录中，搜索由其测试包名称导入的 test\_\*.py 或 \*\_test.py 文件；
	- 从这些文件中，收集如下测试项：
		- test为前缀 的 函数
		- Test为前缀的 类 里面的 test为前缀的方法

操作 | 说明
---- | ----
文件命名 | 用例文件 以 test\_ 开头或 \_test.py 结尾<br>用例类 以 Test 开头<br>用例方法 以 test 开头
断言 | assert 表达式
执行测试 | pytest<br>pytest 用例目录名<br>pytest -sv<br>pytest -sv --html=log/report.html --self-contained-html<br>python -m pytest cases -sv
driver 全局使用 | 定义一个字典类型的全局变量：global_objs = {}<br>将driver对象添加到变量中：global_objs['wd'] = wd<br>获取并使用：wd = global_objs['wd']
导入公共代码 | D:\\workspace\\pyproject\\bysms_test\\pytest\\lib\\funcs.py<br>import sys<br>sys.path.append(r"D:\workspace\pyproject")<br>from bysms_test.pytest.lib.funcs import Funcs

## 用例结构，初始化、还原
```python
import pytest

# 模块标签
pytestmark = [pytest.mark.网页测试, pytest.mark.登录测试]

def setup_module():				# 模块 初始化
    pass


def teardown_module():			# 模块 清除
    pass

@pytest.mark.网页测试			 # 类 标签
@pytest.mark.登陆测试			 
class TestMgrLogin:				# 用例类，以 Test 开头

    @classmethod
    def setup_class(cls):		# 类 初始化
        pass

    @classmethod
    def teardown_class(cls):	# 类 清除
        pass

    def setup_method(self):		# 方法 初始化
        pass

    def teardown_method(self):	# 方法 清除
        pass
    
    @pytest.mark.webtest		# 用例方法 标签
    def test_UI0001(self):		# 用例 UI-0001，以 test_ 开头 或 _test 结尾
        pass
        assert condition		# 检查点，断言

    def test_UI0002(self):		# 用例 UI-0002
        pass
        assert condition
```

目录 初始化、清除：目录下新建 conftest.py，写入下列信息。

```python
import pytest 

@pytest.fixture(scope='package',autouse=True)
def st_emptyEnv():
    print(f'\n#### 初始化-目录甲')
    yield

    print(f'\n#### 清除-目录甲')
```

## 用例执行

```
pytest
pytest cases	# cases 为用例目录
	-v：显示详情
	-s：打印 print() 信息
	-k 用例名：按用例名称挑选用例执行
	--html=./lib/report.html --self-contained-html：输出 html 测试报告
	
# 指定一个模块
	pytest cases\登录\test_错误登录.py
	
# 指定目录
	pytest cases
	pytest cases1 case2

# 指定类、方法
	pytest cases\登录\test_错误登录.py::Test_错误密码
	pytest cases\登录\test_错误登录.py::Test_错误密码::test_C001001
	
# 根据名字
	名字规则：可以是测试函数、类名、模块文件名、目录名，区分大小写，不一定要完整，部分匹配就行。
    pytest -k C001001 -s
    pytest -k "not C001001" -s
    pytest -k "错 and 密码2" -s
    pytest -k "错 or 密码2" -s
    
# 根据标签
	加标签：tagname 支持中文
		import pytest
		@pytest.mark.tagname		
		pytestmark = [pytest.mark.网页测试, pytest.mark.登录测试]
	执行：pytest cases -m tagname -s
	
# 断点调试
    点击打开运行配置
    点击+号， 添加一个运行配置，在右边的输入框输入配置名，比如 pytest
    点击箭头选择 module name，并且输入 pytest 作为运行模块名
    参数输入相应的命令行参数，比如 cases -sv
    工作目录选择项目根目录
    点击 OK
```

## 参数化

```python
@pytest.mark.parametrize(
    'username, password, expectedalert', 
    [('byhy', '88888888', '管理员'),  
    (None, '88888888', '请输入用户名'),  
    ('byhy', None, '请输入密码'),   
    ('byhy', '12345678', '登录失败 : 用户名或者密码错误')],
    ids=["case1", "case2", "case3", "case4"]
    )
def test_0001_0015(self, username, password, expectedalert):  
    assert func.login(username, password) == expectedalert
```

## allure 报告
安装 allure，并添加至系统变量
```cmd
pip install allure-pytest
pip install allure-python-commons

allure --version
```

使用pytest 执行用例时，指定 allure 报告的路径
```python
pytest.main(['-s', '-v', '--alluredir=test_report/allure-results'])
```

cmd 中使用 allure serve 将生成的报告转换为 html 报告
```cmd
allure serve allure-report-path
```

## 调试
当我们需要调试代码时，可以添加断点，然后按照下图所示
![[调试.png]]
1. 点击打开运行配置
2. 点击+号， 添加一个运行配置，在右边的输入框输入配置名，比如 pytest
3. 点击箭头选择 module name，并且输入 pytest 作为运行模块名
4. 参数输入相应的命令行参数，比如 cases -sv
5. 工作目录选择项目根目录
6. 点击 OK

## 案例：web登录
test_login.py
```python
import pytest  
import sys  
sys.path.append(r"D:\workspace\pyproject")  
from bysms_test.pytest.lib.funcs import Funcs  
  
  
def setup_module():  
    print("==== 模块 初始化 ====")  
  
  
def teardown_module():  
    print("==== 模块 清除 ====")  
  
  
func = Funcs()  
  
  
class TestLogin(object):  
    @classmethod  
    def setup_class(cls):  
        print("==== 类 初始化 ====")  
  
    @classmethod  
    def teardown_class(cls):  
        print("==== 类 清除 ====")  
  
    def setup_method(self):  
        print("==== 方法 初始化 ====")  
  
    def teardown_method(self):  
        print("==== 方法 清除 ====")  
        func.alert_accept()  
        func.del_cookies()  
        if func.get_url() == "http://127.0.0.1/mgr/sign.html":  
            func.clear('css selector', '#username')  
            func.clear('css selector', '#password')  
  
    # 数据驱动  
    @pytest.mark.parametrize('username, password, expectedalert', [  
        ('byhy', '88888888', '管理员'),  
        (None, '88888888', '请输入用户名'),  
        ('', '88888888', '请输入用户名'),  
        (' ', '88888888', '登录失败 : 用户名或者密码错误'),  
        ('  byhy  ', '88888888', '登录失败 : 用户名或者密码错误'),  
        ('byh', '88888888', '登录失败 : 用户名或者密码错误'),  
        ('byhyy', '88888888', '登录失败 : 用户名或者密码错误'),  
        ('abcd', '88888888', '登录失败 : 用户名或者密码错误'),  
        ('byhy', None, '请输入密码'),  
        ('byhy', '', '请输入密码'),  
        ('byhy', ' ', '登录失败 : 用户名或者密码错误'),  
        ('byhy', '  88888888  ', '登录失败 : 用户名或者密码错误'),  
        ('byhy', '8888888', '登录失败 : 用户名或者密码错误'),  
        ('byhy', '888888888', '登录失败 : 用户名或者密码错误'),  
        ('byhy', '12345678', '登录失败 : 用户名或者密码错误'),  
    ])  
    def test_0001_0015(self, username, password, expectedalert):  
        assert func.login(username, password) == expectedalert  
  
    def test_login_001(self):  
        """ 正确的数据 """  
        assert func.login('byhy', '88888888') == '管理员'  
  
    def test_login_002(self):  
        """ 用户名错误：为空 """  
        assert func.login(None, '88888888') == "请输入用户名"  
  
    def test_login_003(self):  
        """ 用户名错误：为空格 """  
        assert func.login(' ', '88888888') == '登录失败 : 用户名或者密码错误'  
  
    def test_login_004(self):  
        """ 用户名错误：前后有空格 """  
        assert func.login(' byhy ', '88888888') == '登录失败 : 用户名或者密码错误'  
  
    def test_login_005(self):  
        """ 用户名错误：少一位 """  
        assert func.login('byh', '88888888') == '登录失败 : 用户名或者密码错误'  
  
    def test_login_006(self):  
        """ 用户名错误：多一位 """  
        assert func.login('byhyy', '88888888') == '登录失败 : 用户名或者密码错误'  
  
    def test_login_007(self):  
        """ 密码错误：为空 """  
        assert func.login('byhy', None) == '请输入密码'  
  
    def test_login_008(self):  
        """ 密码错误：为空格 """  
        assert func.login('byhy', ' ') == '登录失败 : 用户名或者密码错误'  
  
    def test_login_009(self):  
        """ 密码错误：位数少一位 """  
        assert func.login('byhy', '8888888') == '登录失败 : 用户名或者密码错误'  
  
    def test_login_010(self):  
        """ 密码错误：组成错误 """  
        assert func.login('byhy', '12345678') == '登录失败 : 用户名或者密码错误'  
  
    def test_login_011(self):  
        """ 密码错误：首尾空格 """  
        assert func.login('byhy', '  88888888 ') == '登录失败 : 用户名或者密码错误'
```

commonlib.py
```python
from selenium import webdriver  
from selenium.common.exceptions import NoAlertPresentException  
  
  
class Commonlib(object):  
  
    def __init__(self):  
        """  
        初始化浏览器
        """
        self.browser = webdriver.Chrome(r"D:\driver\chromedriver.exe")  
        self.browser.maximize_window()  
  
    def open_url(self, url):  
        """  
        打开网址
        :param url:
        :return:  
        """
        self.browser.get(url)  
        self.browser.implicitly_wait(5)  
  
    def __find_element(self, locate_type, value):  
        """  
        查找单个页面元素
        :return:  
        """
        if locate_type == "id":  
            return self.browser.find_element_by_id(value)  
        elif locate_type == "class_name":  
            return self.browser.find_element_by_class_name(value)  
        elif locate_type == "name":  
            return self.browser.find_element_by_name(value)  
        elif locate_type == "tag_name":  
            return self.browser.find_element_by_tag_name(value)  
        elif locate_type == "link_text":  
            return self.browser.find_element_by_link_text(value)  
        elif locate_type == "partial_link_text":  
            return self.browser.find_element_by_partial_link_text(value)  
        elif locate_type == "css selector":  
            return self.browser.find_element_by_css_selector(value)  
        elif locate_type == "xpath":  
            return self.browser.find_element_by_xpath(value)  
  
    def __find_elements(self, locate_type, value):  
        """  
        查找单多个页面元素
        :return:  
        """
        if locate_type == "class_name":  
            return self.browser.find_elements_by_class_name(value)  
        elif locate_type == "name":  
            return self.browser.find_elements_by_name(value)  
        elif locate_type == "tag_name":  
            return self.browser.find_elements_by_tag_name(value)  
        elif locate_type == "link_text":  
            return self.browser.find_elements_by_link_text(value)  
        elif locate_type == "partial_link_text":  
            return self.browser.find_elements_by_partial_link_text(value)  
        elif locate_type == "css selector":  
            return self.browser.find_elements_by_css_selector(value)  
        elif locate_type == "xpath":  
            return self.browser.find_elements_by_xpath(value)  
  
    def input_data(self, locate_type, value, text):  
        """  
        输入数据
        :return:  
        """
        self.__find_element(locate_type, value).send_keys(text)  
  
    def mouse_click(self, locate_type, value):  
        """  
        鼠标单击
        :return:  
        """
        self.__find_element(locate_type, value).click()  
  
    def get_data(self, locate_type, value):  
        """  
        获取断言数据
        :return:  
        """
        return self.__find_element(locate_type, value).text  
  
    def get_alert_text(self):  
        """  
        获取弹框数据
        :return:  
        """
        return self.browser.switch_to.alert.text  
  
    def get_url(self):  
        """  
        获取当前 url
        :return:  
        """
        return self.browser.current_url  
  
    def del_cookies(self):  
        """  
        删除cookie
        :return:  
        """
        self.browser.delete_all_cookies()  
  
    def clear(self, locate_type, value):  
        self.__find_element(locate_type, value).clear()  
  
    def alert_accept(self):  
        has_alert = True  
        try:  
            self.browser.switch_to.alert  
        except NoAlertPresentException:  
            has_alert = False  
  
        if has_alert:  
            self.browser.switch_to.alert.accept()  
  
    def del_obj(self):  
        """  
        手动销毁对象
        :return:  
        """
        self.browser.close()  
  
    def __del__(self):  
        """  
        销毁对象，关闭浏览器及驱动
        :return:  
        """
        try:  
            self.browser.quit()  
        except ImportError:  
            pass
```

funcs.py
```python
from bysms_test.pytest.lib.commonlib import Commonlib  
import time  
from selenium.common.exceptions import NoAlertPresentException  
  
  
class Funcs(Commonlib):  
  
    def login(self, username, password):  
        """  
        bysms 管理员登陆测试：输入账号和密码，点击登陆按钮
        :param username: 管理员账号
        :param password: 管理员密码
        :return:  
        """
        self.open_url("http://127.0.0.1/mgr/sign.html")  
  
        if username is not None:  
            self.input_data('css selector', '#username', username)  
  
        if password is not None:  
            self.input_data('css selector', '#password', password)  
  
        self.mouse_click('css selector', '.btn-flat')  
        time.sleep(1)  
  
        # 两种结果：登陆失败时 alert ，成功时跳转到新页面，此时执行try语句会报异常。  
        try:  
            return self.get_alert_text()  
        except NoAlertPresentException:  
            return self.get_data('css selector', 'span.hidden-xs')  
  
  
if __name__ == '__main__':  
    # Funcs().login('byhy', '88888888')  
    pass
```