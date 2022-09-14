hytest，黑羽test，白月黑羽研发的自动化测试框架，适合做 系统测试 自动化。

## 原理
一个有 teststeps 方法的类，才会被 hytest 当做是一条测试用例。

hytest 的运行分为两个阶段
收集测试用例：这个阶段会搜集所有的用例目录下面的代码中的用例类，把这些类实例化， 从而创建 用例实例对象。
执行测试用例：依次执行 上一步中创建的 用例实例对象。

框架结构

```
hytest-project
	cases
		__init__.py
		__st__.py		# 套件目录的 初始化、清除、标签 的设置文件
		xx.py			# 测试套件
		packagedir		# 包目录，可包含上述3个文件
			__init__.py
			__st__.py
			xx.py
	log
		imgs			# 截图文件
		xx.html			# 测试报告的 html 文件
        xx.log			# 日志文件
    lib
        commonlib.py
        funcs.py
        webui.py
    doc
        说明文档.doc
```

目录结构说明：
自动化测试用例是写在 python 文件中的 一个 python 类，对应一个测试用例文档里面的用例。
cases 目录下面的 每个目录 和 py 文件 都被称之为 测试套件(suite)。测试套件是测试用例的集合 ， 通俗的说，就是一组用例 。
包含 测试用例类 的 python 文件 称之为一个 套件文件。
包含 套件文件的 目录 称之为 测试套件目录。

操作 | 说明
---- | ----
安装、导入 | pip install hytest<br>from hytest import \*
官网教程 | http://www.byhy.net/tut/auto/hytest/01/
编写用例 | class xxx: def teststeps(self): pass
四个方法 | STEP(stepNo:int,desc:str)：在日志和测试报告中打印 **测试步骤说明**<br>INFO(info)：在日志和测试报告中打印 **重要信息**<br>CHECK_POINT(desc:str, condition)：设置每个检查点，**断言**。如果希望某个检查点即使不通过，后续代码仍然继续执行，可以使用参数 failStop=False<br>SELENIUM_LOG_SCREEN(wd, width="")：在日志和测试报告中打印 **截屏图片**
执行用例 | python -m hytest.run<br>hytest<br>hytest --suite<br>hytest --test<br>hytest --tag, --tag "'冒烟测试' and 'UITest'", --tag 冒烟测试 --tag UITest, --tag A\*B<br>hytest --tagnot<br>hytest -A
数据驱动 | 批量写测试数据：ddt_cases = \[{"name": "用例名称", "para": \[para1, para2, ...\]}, {}, ...\]<br>获取测试数据：arg1, arg2, ... = self.para
全局变量 name = '' | 在日志和测试报告中打印 用例名称描述
GSTORE = {} | 用于储存变量，和获取变量<br>储存变量 driver：GSTORE\['driver'\] = driver<br>获取变量 driver：driver = GSTORE\['driver'\]
设置标签 | 设置套件和套件目录的所有用例标签：force_tags = \[\]<br>设置单个用例标签：tags = \[\] 
hytest --new auto1 | 在当前目录下创建一个名为 auto1 的项目目录，包含一个cases目录和一个示例代码文件。

## 用例类
类的 name 属性 指定 用例名，如果没有 name 属性，那么类名就会被当做是用例名称。
类的 **teststeps** 方法 对应 测试步骤 代码。一个类 **必须要有 teststeps 方法**，才会被 hytest 当做是一个测试用例类。
```python
# 建议：类名 对应 用例编号
class UI_0101:
    # 测试用例名字，也建议以用例编号结尾，方便 和 用例文档对应
    # 也方便后面 根据用例名称 挑选执行
    name = '管理员首页 - UI-0101'

    # 测试用例步骤
    def teststeps(self):
```

用例结构

```python
from hytest import *

force_tags = ['套件标签1', '套件标签2', '套件标签n']

def suite_setup():
	pass
	
def suite_teardown():
	pass

class xxx:
	
	name = '用例名称'
	tags = ['用例标签1', '用例标签2', '用例标签n']
	
	def setup(self):			# 用例的 初始化
		pass
	
	def teardown(self):			# 用例的 还原
		pass
	
	def teststeps(self):		# 用例的 操作步骤
		
		STEP(1, '步骤描述')
		pass
		
		STEP(2, '步骤描述')
		pass
		
		STEP(n, '步骤描述')
		pass
		
		INFO(object)
		
		CHECK_POINT('检查点描述', 逻辑表达式)
		
class yyy:
	pass
```

## 初始化和还原
初始化 是 创建环境，清除 是 `还原（或者说销毁）` 环境。
`谁` 做的 `初始化` 操作对环境产生了 `什么改变` ， `谁` 就应该在 `清除` 操作里面做什么样的 `还原` 。
```
# 执行顺序：目录初始化 文件初始化 用例初始化 用例还原 文件还原 目录还原
# 如果 setup 执行失败，则不会执行 teststeps、teardown，如果 teststeps 执行失败，仍会执行 teardown，确保环境被清除还原。

# 单个用例的 初始化和还原：作用域在该用例，在用例类中设置
class xx:
	def setup(self):
		pass
		
	def teardown(self):
		pass
		
# 用例文件的  初始化和还原：作用域在该套件文件中的所有用例，在套件文件中设置
xx.py
	def suite_setup():
        pass

    def suite_teardown():
        pass
        
# 用例目录的 初始化和还原：作用域在整个目录及其子目录中的所有用例，在目录中的 __st__.py 文件中设置
 __st__.py
    def suite_setup():
        pass

    def suite_teardown():
        pass
```

## 执行用例

```
# 执行用例：cases 父目录中 cmd 输入 hytest
    hytest 等同于 python -m hytest.run

# 选择用例执行：支持通配符 *
	# 基于 用例名称 选择用例：
	hytest --test ”casename1“  --test ”casename2“
	hytest --test test*
	hytest --test  *0101
	
	# 基于 套件名称 选择用例：
	hytest --suite "suitename1" --suite "suitename2
	
	# 基于 标签 选择用例：
		# 先加标签
			用例目录标签，force_tags = ['', '']	# 目录下 __st__.py 中添加
			用例文件标签：force_tags = ['', '']	# 用例文件中添加，该文件里面所有测试用例都具有了这些标签。
			单个用例标签：tags = ['', '']			# 用例类中添加，本测试用例就具有了这些标签。
		
		# 再根据标签选择用例
		hytest --tag "tag1" --tag"tag2"
		hytest --tag "tag1" and "tag2"
		hytest --tagnot "tag1"
	
# 使用 参数文件 批量执行
    # 先创建一个名字为 args 的参数文件，内容如下（一行一个参数） 
        --test *0301
        --test *0302
        --test *0303
        --test *1401
        --test *1402
    
	# 再执行参数文件
    hytest -A args
```

## 数据驱动
如果有一批测试用例，具有相同的测试步骤，只是测试数据不同，则可以使用数据驱动（data drivern）来简化代码，把测试数据从用例代码中分离开来，以后增加新的测试用例时，只需要修改数据。

具体步骤
1. 多条用例类合为一个类；
2. 在 ddt_cases = \[{"name": "用例名称", "para": \[para1, para2, ...\]}, {}, ...\] 中定义测试数据，参数会被保存到 self.para 中，必须在测试步骤之前定义；
3. 在 teststeps() 中对 self.para 使用解包获取对应参数数据，即 ”变量1, 变量2, 变量n = self.para“ 。

```python
class UI000x(object):  

    tags = ["管理员登录测试"]  

    # 一个字典定义一条用例的数据，name 用例名称， para 用例参数。hytest执行时会自动创建与字典个数相同的用例实例。
    ddt_cases = [  
        {"name": "正确账号登录成功", "para": ['byhy', '88888888', "管理员"]},  
        {"name": "登录失败（用户名为None）", "para": [None, '88888888', "请输入用户名"]},  
        {"name": "登录失败（用户名为空）", "para": ['', '88888888', '请输入用户名']},  
        {"name": "登录失败（用户名为空格）", "para": [' ', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（用户名前后有空格）", "para": ['  byhy  ', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（用户名少一位）", "para": ['byh', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（用户名多一位）", "para": ['byhyy', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（用户名类型错误）", "para": ['abcd', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码为None）", "para": ['byhy', None, '请输入密码']},  
        {"name": "登录失败（密码为空）", "para": ['byhy', '', '请输入密码']},  
        {"name": "登录失败（密码为空格）", "para": ['byhy', ' ', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码为前后有空格）", "para": ['byhy', '  88888888  ', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码少一位）", "para": ['byhy', '8888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码多一位）", "para": ['byhy', '888888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码类型错误）", "para": ['byhy', '12345678', '登录失败 : 用户名或者密码错误']},  
    ]  
  
    def setup(self):  
        pass  
  
    def teardown(self):  
        INFO('点掉弹框，清除输入，清除COOKIE')  
        func.alert_accept()  
        func.del_cookies()  
        if func.get_url() == "http://127.0.0.1/mgr/sign.html":  
            func.clear_text('css selector', '#username')  
            func.clear_text('css selector', '#password')  
  
    def teststeps(self): 
        # hytest 在调用 teststeps 时，把每个用例的参数设置在 self.para 中，可通过解包获取对应参数。
        username, password, expect = self.para  
  
        CHECK_POINT("", func.login(username, password) == expect)
```

动态产生驱动数据
```python
from hytest import *

class UI_000x:

    ddt_cases = []
    for i in range(10):
        ddt_cases.append({
            'name': f'登录 UI_000{i+1}',
            'para': [None, f'{i+1}'*8,'请输入用户名']
        })
 
    def teststeps(self):
        INFO(f'{self.para}')
```

也可在套件初始化中动态产生
```python
from hytest import *
 
# 套件初始化，只执行一次
def suite_setup():
    GSTORE['ddt_cases_UI_000x'] = []    
    for i in range(10):
        GSTORE['ddt_cases_UI_000x'].append({
            'name': f'登录 UI_000{i+1}',
            'para': [None, f'{i+1}'*8,'请输入用户名']
        }) 
  
class UI_000x:

    ddt_cases = GSTORE['ddt_cases_UI_000x']

    def teststeps(self):
        INFO(f'{self.para}')
```

## 报告中加入图片
`SELENIUM_LOG_SCREEN`：进行截屏并写入测试报告中。

```python
from hytest import *

class c1:
    name = 'web-lesson-0001'

    def teststeps(self):
        self.driver = webdriver.Chrome()
        self.driver.get('http://192.168.56.103/sign.html')

        # 第1个参数是 webdriver 对象
        # width 参数为可选参数， 指定图片显示宽度
        SELENIUM_LOG_SCREEN(driver, width='70%') 
```

`LOG_IMG`：直接在报告中 插入 已经产生的图片。

```python
from hytest import *

class c1:
    name = 'web-lesson-0001'

    def teststeps(self):

        # 第1个参数是 图片路径，可以是网络url 
        LOG_IMG('http://www.byhy.net/xxx.png')
        # 也可以是相对报告文件的本地路径
        LOG_IMG('imgs/abc.png')
        # width 参数为可选参数， 指定图片显示宽度
        LOG_IMG('imgs/abc.png', width='70%')
```

## 调式
PyCharm中调试
1. Run - Edit Configurations - + - Python；
2. 设置 Name；
3. Configuration 中 将 Script path 替换选择为 Module name，输入 hytest.run；
4. Parameters 输入 要调试的用例如 --test "casename"；
5. Working directory 输入项目地址如”D:\\workspace\\pyproject\\bysms_test\\hytest“；
6. debug 调试。
![[hytest调试.png]]

## 案例：bysms
### 用例 + 类方法 + 类通用方法
test_add.py
```python
from hytest import *  
import time  
import sys  
sys.path.append(r'D:\workspace\pyproject')  
from bysms_test.hytest.lib.funcs import Funcs  
  
force_tags = ["管理员操作"]  
func = Funcs()  
  
  
# 套件初始化  
def suite_setup():  
    INFO("登录")  
    func.login('byhy', '88888888')  
  
  
def suite_teardown():  
    INFO("退出登录")  
    func.logout()  
  
  
class UI0101:  
  
    name = "检查左侧菜单前三项 - UI-0101"  
  
    # 用例初始化  
    def setup(self):  
        pass  
  
    def teardown(self):  
        pass  
  
    # 用例步骤  
    def teststeps(self):  
  
        STEP(1, '获取左侧菜单项')  
        menu_list = func.get_data_list('xpath', '//ul[@class="sidebar-menu tree"]/li[position()>1]')  
        INFO(menu_list)  
  
        STEP(2, '检查菜单前三项')  
        CHECK_POINT('', menu_list[:3] == ["客户", "药品", "订单"])  
  
  
class UI0102:  
  
    name = "添加客户 - UI-0102"  
    tags = ["添加客户"]  
  
    def setup(self):  
        pass  
        # clear_all()  
  
    def teardown(self):  
        INFO('删掉新添加客户')  
        customers = GSTORE['customers']  
        func.del_new("customers", len(customers))  
  
    def teststeps(self):  
  
        customers = [["南京中医院1", "2551867850", "江苏省-南京市-秦淮区-汉中路-500"],  
                     ['南京中医院2', '2551867852', '江苏省-南京市-秦淮区-汉中路-502'],  
                     ['南京中医院3', '2551867853', '江苏省-南京市-秦淮区-汉中路-503'],  
                     ['南京中医院4', '2551867854', '江苏省-南京市-秦淮区-汉中路-504'],  
                     ['南京中医院5', '2551867855', '江苏省-南京市-秦淮区-汉中路-505'],  
                     ['南京中医院6', '2551867856', '江苏省-南京市-秦淮区-汉中路-506'],  
                     ['南京中医院7', '2551867857', '江苏省-南京市-秦淮区-汉中路-507'],  
                     ['南京中医院8', '2551867858', '江苏省-南京市-秦淮区-汉中路-508'],  
                     ['南京中医院9', '2551867859', '江苏省-南京市-秦淮区-汉中路-509'],  
                     ['南京中医院10', '2551867860', '江苏省-南京市-秦淮区-汉中路-510'],  
                     ['南京中医院11', '2551867861', '江苏省-南京市-秦淮区-汉中路-511']]  
        GSTORE['customers'] = customers  
  
        STEP(1, '切换至 客户 菜单')  
        func.click_element('xpath', '//a[contains(@href, "customers")]')  
  
        STEP(2, '添加客户')  
        func.input_data("customers", customers)  
  
        STEP(3, '获取结果列表中的新客户信息')  
        time.sleep(0.5)  
        res_list = func.get_result_item("customers", len(customers))  
        INFO(res_list)  
  
        STEP(4, '检查结果列表中的详细信息')  
        CHECK_POINT('结果和预期一致', res_list == customers[::-1])  
  
  
class UI0108:  
  
    name = '添加 药品、客户、订单 - UI-0108'  
    tags = ["添加药品、客户、订单"]  
  
    def setup(self):  
        INFO('清空已有的所有 订单、客户、药品')  
        func.clear_all()  
  
    def teardown(self):  
        INFO('删掉 新添加内容：订单、客户、药品')  
        orders = GSTORE['orders']  
        customers = GSTORE['customers']  
        medicines = GSTORE['medicines']  
        func.del_new("orders", len(orders))  
        func.del_new("customers", len(customers))  
        func.del_new("medicines", len(medicines))  
  
    def teststeps(self):  
  
        medicines = [['青霉素盒装1', 'YP-32342341', '青霉素注射液，每支15ml，20支装'],  
                     ['青霉素盒装2', 'YP-32342342', '青霉素注射液，每支15ml，30支装'],  
                     ['青霉素盒装3', 'YP-32342343', '青霉素注射液，每支15ml，40支装'],  
                     ['青霉素盒装4', 'YP-32342344', '青霉素注射液，每支15ml，50支装'],  
                     ['青霉素盒装5', 'YP-32342345', '青霉素注射液，每支15ml，60支装'],  
                     ['青霉素盒装6', 'YP-32342346', '青霉素注射液，每支15ml，70支装']]  
        customers = [["南京中医院1", "2551867850", "江苏省-南京市-秦淮区-汉中路-500"],  
                     ['南京中医院2', '2551867852', '江苏省-南京市-秦淮区-汉中路-502'],  
                     ['南京中医院3', '2551867853', '江苏省-南京市-秦淮区-汉中路-503'],  
                     ['南京中医院4', '2551867854', '江苏省-南京市-秦淮区-汉中路-504'],  
                     ['南京中医院5', '2551867855', '江苏省-南京市-秦淮区-汉中路-505'],  
                     ['南京中医院6', '2551867856', '江苏省-南京市-秦淮区-汉中路-506']]  
        orders = [['南京中医院1', '青霉素盒装1', '100'],  
                  ['南京中医院2', '青霉素盒装2', '200'],  
                  ['南京中医院3', '青霉素盒装3', '300'],  
                  ['南京中医院4', '青霉素盒装4', '400'],  
                  ['南京中医院5', '青霉素盒装5', '500'],  
                  ['南京中医院6', '青霉素盒装6', '600']]  
        GSTORE['orders'] = orders  
        GSTORE['customers'] = customers  
        GSTORE['medicines'] = medicines  
  
        STEP(1, '添加 药品、客户、订单')  
        for e in ["customers", "medicines", "orders"]:  
            func.input_data(e, eval(e))  
            SELENIUM_LOG_SCREEN(func.browser, width="60%")  
  
        STEP(2, '获取新添加内容：药品、客户、订单')  
        time.sleep(0.5)  
        customers_res_item = func.get_result_item("customers", len(customers))  
        medicines_res_item = func.get_result_item("medicines", len(medicines))  
        orders_res_item = func.get_result_item("orders", len(orders))  
  
        INFO(customers_res_item)  
        INFO(medicines_res_item)  
        INFO(orders_res_item)  
  
        STEP(3, '检查新添加内容')  
        CHECK_POINT('新添加客户与预期一致', customers_res_item == customers[::-1])  
        CHECK_POINT('新添加药品与预期一致', medicines_res_item == medicines[::-1])  
        CHECK_POINT('新添加订单与预期一致', orders_res_item == orders[::-1])
```

test_login.py
```python
from hytest import *  
import sys  
sys.path.append(r"D:\workspace\pyproject")  
from bysms_test.hytest.lib.funcs import Funcs  
from selenium.common.exceptions import NoAlertPresentException  
  
force_tags = ["登录测试", "冒烟测试", "UI测试"]  
  
func = Funcs()  
  
  
class UI000x(object):  
  
    tags = ["管理员登录测试"]  
  
    ddt_cases = [  
        {"name": "正确账号登录成功", "para": ['byhy', '88888888', "管理员"]},  
        {"name": "登录失败（用户名为None）", "para": [None, '88888888', "请输入用户名"]},  
        {"name": "登录失败（用户名为空）", "para": ['', '88888888', '请输入用户名']},  
        {"name": "登录失败（用户名为空格）", "para": [' ', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（用户名前后有空格）", "para": ['  byhy  ', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（用户名少一位）", "para": ['byh', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（用户名多一位）", "para": ['byhyy', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（用户名类型错误）", "para": ['abcd', '88888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码为None）", "para": ['byhy', None, '请输入密码']},  
        {"name": "登录失败（密码为空）", "para": ['byhy', '', '请输入密码']},  
        {"name": "登录失败（密码为空格）", "para": ['byhy', ' ', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码为前后有空格）", "para": ['byhy', '  88888888  ', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码少一位）", "para": ['byhy', '8888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码多一位）", "para": ['byhy', '888888888', '登录失败 : 用户名或者密码错误']},  
        {"name": "登录失败（密码类型错误）", "para": ['byhy', '12345678', '登录失败 : 用户名或者密码错误']},  
    ]  
  
    def setup(self):  
        pass  
  
    def teardown(self):  
        INFO('点掉弹框，清除输入，清除COOKIE')  
        func.alert_accept()  
        func.del_cookies()  
        if func.get_url() == "http://127.0.0.1/mgr/sign.html":  
            func.clear_text('css selector', '#username')  
            func.clear_text('css selector', '#password')  
  
    def teststeps(self):  
        username, password, expect = self.para  
  
        CHECK_POINT("", func.login(username, password) == expect)  
```

funcs.py
```python
# 测试点库：各个测试点的操作步骤  
from bysms_test.hytest.lib.commonlib import Commonlib  
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
            self.input_text('css selector', '#username', username)  
  
        if password is not None:  
            self.input_text('css selector', '#password', password)  
  
        self.click_element('css selector', '.btn-flat')  
        time.sleep(1)  
  
        # 两种结果：登陆失败时 alert ，成功时跳转到新页面，此时执行try语句会报异常。  
        try:  
            return self.get_alert_text()  
        except NoAlertPresentException:  
            return self.get_data('css selector', 'span.hidden-xs')  
  
    def logout(self):  
        self.click_element('xpath', '//ul[@class="nav navbar-nav"]/li[2]')  
        self.click_element('xpath', '//ul[@class="nav navbar-nav"]/li[2] //div[@class="pull-right"]')  
  
    def input_data(self, dataitem, data_list):  
        """  
        输入 客户、药品、订单。一组元素 分别输入 二维列表中的对应文本
        :param dataitem: 客户 "customers" | 药品 "medicines" | 订单 "orders" 
        :param data_list: 输入内容的二维列表
        :return:  
        """
        # 点击左侧菜单栏，切换到对应项  
        self.click_element('xpath', '//a[contains(@href, "%s")]' % dataitem)  
  
        # 点击 添加客户 | 添加药品 | 添加订单  
        self.click_element('xpath', '//button[@class="btn btn-green btn-outlined btn-md"]')  
  
        # 循环添加：依次输入、点击创建  
        for i in range(len(data_list)):  
            if dataitem in ["customers", "medicines"]:  
                j = 0  
                for e in self.get_elements('xpath', '//div[@class="col-lg-8 col-md-8 col-sm-8"] //*[@class="form-control"]'):  
                    e.send_keys(data_list[i][j])  
                    j += 1  
                self.click_element('xpath', '//div[@class="col-lg-12 col-md-12 col-sm-12"]/button[1]')  
                time.sleep(0.1)  
            elif dataitem == "orders":  
                # 输入 订单名称  
                self.get_elements('xpath', '//div[@class="col-lg-8 col-md-8 col-sm-8"] //*[@class="form-control"]')[  
                    0].send_keys("%s_%s_%s" % (data_list[i][0], data_list[i][1], data_list[i][2]))  
  
                # 选择 客户和药品  
                select_li = self.get_elements('xpath', '//div[@class="col-lg-8 col-md-8 col-sm-8"] //select[@class="xxx"]')  
  
                self.select_opt(select_li[0], data_list[i][0])  
                self.select_opt(select_li[1], data_list[i][1])  
  
                # 输入 数量  
                time.sleep(0.1)  
                self.input_text('xpath', '//*[@id="root"]/div/section[2]/div[1]/div[1]/div[3]/div/input', data_list[i][2])  
  
                # 点击 创建  
                self.click_element('xpath', '//div[@class="col-lg-12 col-md-12 col-sm-12"]/button[1]')  
                time.sleep(0.1)  
  
    def get_result_item(self, dataitem, num):  
        """  
        从结果显示列表中首页第一条开始，逐条往后获取，最大获取条数为新添加内容条数，作为循环次数。仅限 客户、药品。
        :param dataitem: 客户 "customers" | 药品 "medicines" | 订单 "orders"
        :param num: 循环次数 = 最大获取条数 = 新添加内容条数
        :return: 返回一个 循环获取到的 关于新添加内容的 列表  
        """
        # 点击左侧菜单栏，切换到对应项  
        self.click_element('xpath', '//a[contains(@href, "%s")]' % dataitem)  
  
        res_list = []  
        pos = 1  
        for i in range(num):  
            if dataitem == "orders":  
                xpath1 = '//div[@class="search-result-item"][position()="%d"]' % pos  
                xpath = xpath1 + '/div[3]/span[2]' + ' | ' + xpath1 + '/div[4]/p'  
                res_list.append(' * '.join(self.get_data_list('xpath', xpath)).split(' * '))  
            else:  
                xpath = '//div[@class="search-result-item"][position()="%d"] //span[last()]' % pos  
                res_list.append(self.get_data_list('xpath', xpath))  
            pos += 1  
            # 一页限制 5 条结果显示，多的需要翻页  
            if pos % 5 == 1:  
                # 点击 下一页 的图标按钮，等待内容出现  
                self.click_element('xpath', '//ul[@class="pagination"]/li[last()-1]/a')  
                time.sleep(0.1)  
                pos -= 5  
        return res_list  
  
    def del_new(self, dataitem, num):  
        """  
        删除新添加的内容：订单、客户、药品
        :param dataitem: 订单 orders、客户 customers、药品 medicines
        :param num: 删除条数
        :return:  
        """
        # 点击左侧菜单栏，切换到对应项  
        self.click_element('xpath', '//a[contains(@href, "%s")]' % dataitem)  
  
        # 先保证在 页码的首页：如果当前页码不是第1页，就点击 首页。  
        if self.get_data('xpath', '//li[@class="active"]/a') != "1":  
            self.click_elements('xpath', '//ul[@class="pagination"]/li[1]/a')  
            time.sleep(0.1)  
  
        for i in range(num):  
            self.click_element('xpath', '//div[@class="search-result-item"][1] //label[last()]')  
            self.alert_accept()  
            time.sleep(0.1)  
  
    def clear_all(self):  
        # 先删除 订单 的引用，才能删除被引用的 客户、药品  
        for li in ["orders", "customers", "medicines"]:  
            self.click_element('xpath', '//a[contains(@href, "%s")]' % li)  
            # 找得到 结果列表，则循环删除，找不到退出内层循环  
            while self.get_elements('xpath', '//div[@class="search-result-item"]'):  
                self.click_element('xpath', '//div[@class="search-result-item"] //label[last()]')  
                self.alert_accept()  
                time.sleep(0.1)  
            # while True:  
            #     if not self.get_elements('xpath', '//div[@class="search-result-item"]'):
            #         break
            #     else:
            #         self.click_element('xpath', '//div[@class="search-result-item"] //label[last()]')
            #         self.alert_accept()
            #         time.sleep(0.1)  
  
if __name__ == '__main__':  
    # func = Funcs()  
    # func.login('byhy', '88888888')
    # time.sleep(1)
    # func.logout()
    pass
```

commonlib.py
```python
from selenium import webdriver  
from selenium.common.exceptions import NoAlertPresentException  
from selenium.webdriver.support.select import Select  
  
  
class Commonlib(object):  
  
    def __init__(self):  
        """  
        初始化浏览器
        """
        options = webdriver.ChromeOptions()  
        options.add_experimental_option('excludeSwitches', ['enable-logging'])  
        self.browser = webdriver.Chrome(r"D:\driver\chromedriver.exe", options=options)  
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
  
    def get_elements(self, locate_type, value):  
        return self.__find_elements(locate_type, value)  
  
    def input_text(self, locate_type, value, text):  
        """  
        输入数据
        :return:  
        """
        self.__find_element(locate_type, value).send_keys(text)  
  
    def clear_text(self, locate_type, value):  
        self.__find_element(locate_type, value).clear()  
  
    def click_element(self, locate_type, value):  
        self.__find_element(locate_type, value).click()  
  
    def click_elements(self, locate_type, value):  
        for element in self.__find_elements(locate_type, value):  
            element.click()  
  
    def get_data(self, locate_type, value):  
        """  
        获取断言数据
        :return:  
        """
        return self.__find_element(locate_type, value).text  
  
    def get_data_list(self, locate_type, value):  
        return [e.text for e in self.__find_elements(locate_type, value)]  
  
    def get_alert_text(self):  
        """  
        获取弹框数据
        :return:  
        """
        return self.browser.switch_to.alert.text  
  
    def select_opt(self, sel_e, by):  
        """  
        select 下拉选择框
        :param sel_e: 定位的select元素
        :param by: 选择依据
        :return:  
        """
        sel = Select(sel_e)  
        if sel.is_multiple:  
            sel.deselect_all()  
        sel.select_by_visible_text(by)  
  
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

### 用例文件 + 方法文件
autotest.py
```python
from hytest import *  
import time  
import sys  
sys.path.append(r'D:\workspace\pyproject')  
from bysms_test.hytest.lib.webui import *  
  
force_tags = ["管理员操作"]  
  
  
# 套件初始化  
def suite_setup():  
    INFO("打开浏览器")  
    open_browser()  
  
    INFO("登录")  
    mgr_login('http://127.0.0.1/mgr/sign.html', 'byhy', '88888888')  
  
  
def suite_teardown():  
    INFO("退出登录")  
    mgr_logout()  
  
    INFO("关闭浏览器")  
    quit_browser()  
  
  
class UI0101:  
  
    name = "检查左侧菜单前三项 - UI-0101"  
  
    # 用例初始化  
    def setup(self):  
        pass  
  
    def teardown(self):  
        pass  
  
    # 用例步骤  
    def teststeps(self):  
  
        STEP(1, '获取左侧菜单项')  
        menu_list = get_text_list('xpath', '//ul[@class="sidebar-menu tree"]/li[position()>1]')  
        INFO(menu_list)  
  
        STEP(2, '检查菜单前三项')  
        CHECK_POINT('', menu_list[:3] == ["客户", "药品", "订单"])  
  
  
class UI0102:  
  
    name = "添加客户 - UI-0102"  
    tags = ["添加客户"]  
  
    def setup(self):  
        pass  
        # clear_all()  
  
    def teardown(self):  
        INFO('删掉新添加客户')  
        customers = GSTORE['customers']  
        del_new("customers", len(customers))  
  
    def teststeps(self):  
  
        customers = [["南京中医院1", "2551867850", "江苏省-南京市-秦淮区-汉中路-500"],  
                     ['南京中医院2', '2551867852', '江苏省-南京市-秦淮区-汉中路-502'],  
                     ['南京中医院3', '2551867853', '江苏省-南京市-秦淮区-汉中路-503'],  
                     ['南京中医院4', '2551867854', '江苏省-南京市-秦淮区-汉中路-504'],  
                     ['南京中医院5', '2551867855', '江苏省-南京市-秦淮区-汉中路-505'],  
                     ['南京中医院6', '2551867856', '江苏省-南京市-秦淮区-汉中路-506'],  
                     ['南京中医院7', '2551867857', '江苏省-南京市-秦淮区-汉中路-507'],  
                     ['南京中医院8', '2551867858', '江苏省-南京市-秦淮区-汉中路-508'],  
                     ['南京中医院9', '2551867859', '江苏省-南京市-秦淮区-汉中路-509'],  
                     ['南京中医院10', '2551867860', '江苏省-南京市-秦淮区-汉中路-510'],  
                     ['南京中医院11', '2551867861', '江苏省-南京市-秦淮区-汉中路-511']]  
        GSTORE['customers'] = customers  
  
        STEP(1, '切换至 客户 菜单')  
        click_element('xpath', '//a[contains(@href, "customers")]')  
  
        STEP(2, '添加客户')  
        input_data("customers", customers)  
  
        STEP(3, '获取结果列表中的新客户信息')  
        time.sleep(0.5)  
        res_list = get_result_item("customers", len(customers))  
        INFO(res_list)  
  
        STEP(4, '检查结果列表中的详细信息')  
        CHECK_POINT('结果和预期一致', res_list == customers[::-1])  
  
  
class UI0108:  
  
    name = '添加 药品、客户、订单 - UI-0108'  
    tags = ["添加药品、客户、订单"]  
  
    def setup(self):  
        INFO('清空已有的所有 订单、客户、药品')  
        clear_all()  
  
    def teardown(self):  
        INFO('删掉 新添加内容：订单、客户、药品')  
        orders = GSTORE['orders']  
        customers = GSTORE['customers']  
        medicines = GSTORE['medicines']  
        del_new("orders", len(orders))  
        del_new("customers", len(customers))  
        del_new("medicines", len(medicines))  
  
    def teststeps(self):  
  
        medicines = [['青霉素盒装1', 'YP-32342341', '青霉素注射液，每支15ml，20支装'],  
                     ['青霉素盒装2', 'YP-32342342', '青霉素注射液，每支15ml，30支装'],  
                     ['青霉素盒装3', 'YP-32342343', '青霉素注射液，每支15ml，40支装'],  
                     ['青霉素盒装4', 'YP-32342344', '青霉素注射液，每支15ml，50支装'],  
                     ['青霉素盒装5', 'YP-32342345', '青霉素注射液，每支15ml，60支装'],  
                     ['青霉素盒装6', 'YP-32342346', '青霉素注射液，每支15ml，70支装']]  
        customers = [["南京中医院1", "2551867850", "江苏省-南京市-秦淮区-汉中路-500"],  
                     ['南京中医院2', '2551867852', '江苏省-南京市-秦淮区-汉中路-502'],  
                     ['南京中医院3', '2551867853', '江苏省-南京市-秦淮区-汉中路-503'],  
                     ['南京中医院4', '2551867854', '江苏省-南京市-秦淮区-汉中路-504'],  
                     ['南京中医院5', '2551867855', '江苏省-南京市-秦淮区-汉中路-505'],  
                     ['南京中医院6', '2551867856', '江苏省-南京市-秦淮区-汉中路-506']]  
        orders = [['南京中医院1', '青霉素盒装1', '100'],  
                  ['南京中医院2', '青霉素盒装2', '200'],  
                  ['南京中医院3', '青霉素盒装3', '300'],  
                  ['南京中医院4', '青霉素盒装4', '400'],  
                  ['南京中医院5', '青霉素盒装5', '500'],  
                  ['南京中医院6', '青霉素盒装6', '600']]  
        GSTORE['orders'] = orders  
        GSTORE['customers'] = customers  
        GSTORE['medicines'] = medicines  
  
        STEP(1, '添加 药品、客户、订单')  
        wd = GSTORE['wd']  
        for e in ["customers", "medicines", "orders"]:  
            input_data(e, eval(e))  
            SELENIUM_LOG_SCREEN(wd, width="60%")  
  
        STEP(2, '获取新添加内容：药品、客户、订单')  
        time.sleep(0.5)  
        customers_res_item = get_result_item("customers", len(customers))  
        medicines_res_item = get_result_item("medicines", len(medicines))  
        orders_res_item = get_result_item("orders", len(orders))  
  
        INFO(customers_res_item)  
        INFO(medicines_res_item)  
        INFO(orders_res_item)  
  
        STEP(3, '检查新添加内容')  
        CHECK_POINT('新添加客户与预期一致', customers_res_item == customers[::-1])  
        CHECK_POINT('新添加药品与预期一致', medicines_res_item == medicines[::-1])  
        CHECK_POINT('新添加订单与预期一致', orders_res_item == orders[::-1])
```

webui.py
```python
from selenium import webdriver  
import time  
from hytest import *  
from selenium.webdriver.support.select import Select  
  
  
def open_browser():  
    options = webdriver.ChromeOptions()  
    options.add_experimental_option('excludeSwitches', ['enable-logging'])  
    wd = webdriver.Chrome(r"D:\driver\chromedriver.exe", options=options)  
    wd.implicitly_wait(5)
    GSTORE['wd'] = wd  
  
  
def quit_browser():  
    wd = GSTORE['wd']  
    wd.quit()  
  
  
def open_url(url):  
    wd = GSTORE['wd']  
    wd.get(url)   
  
  
def mgr_login(url, username, password):  
    wd = GSTORE['wd']  
    wd.get(url)  
    wd.find_element_by_css_selector('#username').send_keys(username)  
    wd.find_element_by_css_selector('#password').send_keys(password)  
    wd.find_element_by_css_selector(".btn-flat").click()  
  
  
def mgr_logout():  
    # 点击 右上导航 管理员，点击 退出登录  
    click_element('xpath', '//ul[@class="nav navbar-nav"]/li[2]')  
    click_element('xpath', '//ul[@class="nav navbar-nav"]/li[2] //div[@class="pull-right"]')  
  
  
def find_elements(locate_type, value):  
    """  
    查找一组元素
    """
    wd = GSTORE['wd']  
    if locate_type == "css_selector":  
        return wd.find_elements_by_css_selector(value)  
    elif locate_type == "xpath":  
        return wd.find_elements_by_xpath(value)  
    elif locate_type == "class":  
        return wd.find_elements_by_class_name(value)  
    elif locate_type == "name":  
        return wd.find_elements_by_name(value)  
    elif locate_type == "tag":  
        return wd.find_elements_by_tag_name(value)  
    elif locate_type == "link":  
        return wd.find_elements_by_link_text(value)  
    elif locate_type == "partial_link":  
        return wd.find_elements_by_partial_link_text(value)  
  
  
def find_element(locate_type, value):  
    """  
    查找一个元素
    """
    wd = GSTORE['wd']  
    if locate_type == "id":  
        return wd.find_element_by_id(value)  
    elif locate_type == "class":  
        return wd.find_element_by_class_name(value)  
    elif locate_type == "name":  
        return wd.find_element_by_name(value)  
    elif locate_type == "tag":  
        return wd.find_element_by_tag_name(value)  
    elif locate_type == "link":  
        return wd.find_element_by_link_text(value)  
    elif locate_type == "partial_link":  
        return wd.find_element_by_partial_link_text(value)  
    elif locate_type == "css_selector":  
        return wd.find_element_by_css_selector(value)  
    elif locate_type == "xpath":  
        return wd.find_element_by_xpath(value)  
  
  
def get_text_list(locate_type, value):  
    return [e.text for e in find_elements(locate_type, value)]  
  
  
def get_text(locate_type, value):  
    return find_element(locate_type, value).text  
  
  
def get_alert_text():  
    wd = GSTORE['wd']  
    return wd.switch_to.alert.text  
  
  
def accept_alert():  
    wd = GSTORE['wd']  
    wd.switch_to.alert.accept()  
  
  
def click_elements(locate_type, value):  
    for element in find_elements(locate_type, value):  
        element.click()  
  
  
def click_element(locate_type, value):  
    find_element(locate_type, value).click()  
  
  
def input_text(locate_type, value, text):  
    find_element(locate_type, value).send_keys(text)  
  
  
def clear_text(locate_type, value):  
    find_element(locate_type, value).clear()  
  

def select_opt(sel_e, by):  
    """  
    select 下拉选择框
    :param sel_e: 定位的select元素
    :param by: 选择依据
    :return:  
    """
    sel = Select(sel_e)  
    if sel.is_multiple:  
        sel.deselect_all()  
    sel.select_by_visible_text(by)


def input_data(datatype, data_list):  
    """  
    输入 客户、药品、订单。一组元素 分别输入 二维列表中的对应文本
    :param datatype: 客户 "customers" | 药品 "medicines" | 订单 "orders"
    :param data_list: 输入内容的二维列表
    :return:  
    """
    # 点击左侧菜单栏，切换到对应项  
    click_element('xpath', '//a[contains(@href, "%s")]' % datatype)  
  
    # 点击 添加客户 | 添加药品 | 添加订单  
    click_element('xpath', '//button[@class="btn btn-green btn-outlined btn-md"]')  
  
    # 循环添加：依次输入、点击创建  
    for i in range(len(data_list)):  
        if datatype in ["customers", "medicines"]:  
            j = 0  
            for element in find_elements('xpath', '//div[@class="col-lg-8 col-md-8 col-sm-8"] //*[@class="form-control"]'):  
                element.send_keys(data_list[i][j])  
                j += 1  
            click_element('xpath', '//div[@class="col-lg-12 col-md-12 col-sm-12"]/button[1]')  
            time.sleep(0.1)  
        elif datatype == "orders":  
            # 输入 订单名称  
            find_elements('xpath', '//div[@class="col-lg-8 col-md-8 col-sm-8"] //*[@class="form-control"]')[  
                0].send_keys("%s_%s_%s" % (data_list[i][0], data_list[i][1], data_list[i][2]))  
  
            # 选择 客户、药品  
            select_list = find_elements('xpath', '//div[@class="col-lg-8 col-md-8 col-sm-8"] //select[@class="xxx"]')  
            select_opt(select_list[0], data_list[i][0])  
            select_opt(select_list[1], data_list[i][1])
            
            # 输入 数量  
            time.sleep(0.1)  
            input_text('xpath', '//*[@id="root"]/div/section[2]/div[1]/div[1]/div[3]/div/input', data_list[i][2])  
  
            # 点击 创建  
            click_element('xpath', '//div[@class="col-lg-12 col-md-12 col-sm-12"]/button[1]')  
            time.sleep(0.1)  
  
  
def get_result_item(datatype, num):  
    """  
    从结果显示列表中首页第一条开始，逐条往后获取，最大获取条数为新添加内容条数，作为循环次数。仅限 客户、药品。
    :param datatype: 客户 "customers" | 药品 "medicines" | 订单 "orders"
    :param num: 循环次数 = 最大获取条数 = 新添加内容条数
    :return: 返回一个 循环获取到的 关于新添加内容的 列表  
    """
    # 点击左侧菜单栏，切换到对应项  
    click_element('xpath', '//a[contains(@href, "%s")]' % datatype)  
  
    res_list = []  
    pos = 1  
    for i in range(num):  
        if datatype == "orders":  
            xpath1 = '//div[@class="search-result-item"][position()="%d"]' % pos  
            xpath = xpath1 + '/div[3]/span[2]' + ' | ' + xpath1 + '/div[4]/p'  
            res_list.append(' * '.join(get_text_list('xpath', xpath)).split(' * '))  
        else:  
            xpath = '//div[@class="search-result-item"][position()="%d"] //span[last()]' % pos  
            res_list.append(get_text_list('xpath', xpath))  
        pos += 1  
        # 一页限制 5 条结果显示，多的需要翻页  
        if pos % 5 == 1:  
            # 点击 下一页 的图标按钮，等待内容出现  
            click_element('xpath', '//ul[@class="pagination"]/li[last()-1]/a')  
            time.sleep(0.1)  
            pos -= 5  
    return res_list  
  
  
def del_new(type, num):  
    """  
    删除新添加的内容：订单、客户、药品
    :param type: 订单 orders、客户 customers、药品 medicines
    :param num: 删除条数
    :return:  
    """
    # 点击左侧菜单栏，切换到对应项  
    click_element('xpath', '//a[contains(@href, "%s")]' % type)  
  
    # 先保证在 页码的首页：如果当前页码不是第1页，就点击 首页。  
    if get_text('xpath', '//li[@class="active"]/a') != "1":  
        click_elements('xpath', '//ul[@class="pagination"]/li[1]/a')  
        time.sleep(0.1)  
  
    for i in range(num):  
        click_element('xpath', '//div[@class="search-result-item"][1] //label[last()]')  
        accept_alert()  
        time.sleep(0.1)  
  
  
def clear_all():  
    # 先删除 订单 的引用，才能删除被引用的 客户、药品  
    for li in ["orders", "customers", "medicines"]:  
        click_element('xpath', '//a[contains(@href, "%s")]' % li)  
        # 找得到 结果列表，则循环删除，找不到退出内层循环  
        while find_elements('xpath', '//div[@class="search-result-item"]'):  
            click_element('xpath', '//div[@class="search-result-item"] //label[last()]')  
            accept_alert()  
            time.sleep(0.1)  
        # while True:  
        #     if not find_elements('xpath', '//div[@class="search-result-item"]'):
        #         break
        #     else:
        #         click_element('xpath', '//div[@class="search-result-item"] //label[last()]')
        #         accept_alert()
        #         time.sleep(0.1)
```