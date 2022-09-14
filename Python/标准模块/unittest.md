UnitTest，单元测试模块。可用于功能自动化测试。

原理：继承自 unittest.TestCase 类的子类中，命名以 test 开头的方法会被框架自动识别为测试用例，用例中使用框架提供的断言方法进行断言。

unittest 在自动化测试中的作用
1，继承 unittest.TestCase 类，以调用其断言方法，以调用 Fixture 如 setUp()、tearDown() 设置用例、类、模块执行前后的动作。	
2，使用套件组织用例：unittest.TestSuite().addTest() 或 unittest.defaultTestLoader.discover()。
3，执行用例，输出报告：unittest.main() 或 unittest.TextTestRunner().run()。

## 框架结构
- import unittest
    - unittest.TestCase
        - setUpModule()/tearDownMdodule()
        - class testCaseClass(unittest.TestCase): pass
            - def test_xxx(self): pass
                - self.assertEqual(), ...
            - setUpClass(cls)/tearDownClass(cls)
            - setUp(self)/tearDown(self)
    - unittest.TestSuite
        - suites = unittest.TestSuite()
        - suites.addTest()
            - loader = unittest.TestLoader()
            - suites.addTest(loader.loadTestsFromTestCase(testCaseClass))
            - suites.addTest(loader.loadTestsFromModule(module))
            - suites.addTest(loader.discover())
        - suites = unittest.defaultTestLoader.discover()
    - unittest.TextTestRunner(verbosity=2).run(suites)
    - unittest.main()

![[HTMLTestRunner.py]]

编写要点
1. 测试点函数模块（一步一行，需要调用功能函数模块，继承通用功能函数模块，使用其中的方法实现步骤。）；
2. 通用功能函数模块（一个函数只管一个基础功能，支撑测试点函数模块。如selenium.webdriver操作具体网页元素。）；
3. 测试用例模块（三行。1-实例化对象，2-调用测试点函数，引入测试数据；3-断言预期与实际结果的一致性）；
    - 继承 unittest.TestCase 类
    - 测试用例是名称以‘test'开头的方法
    - 使用该类中的断言方法如 assertEqual(a, b)、assertTrue(x)等进行断言
    - setUp()、tearDown() 方法设置用例执行前后的预处理和清理
4. 套件执行与测试报告：指定用例批量执行，生成测试报告。
    - 使用套件组织用例：unittest.TestSuite().addTest()，unittest.defaultTestLoader.discover()
    - 输出自带测试报告：unittest.TextTestRunner(verbosity=2).run(my_case_suite) 
    - 输出网页测试报告：导入第三方模块 HTMLTestRunner.py，创建html文件并写入测试报告

四大组件

UnitTest 工作流中核心的四大组件简介：
（1）Test Fixture 是指在执行测试之前的准备工作，比如数据清理工作、创建临时数据库、目录，以及开启某些服务进程等。
（2）Test Case 是最小的测试单元，具有独立性。主要检测输出结果是否满足期望，这些结果基于一系列特定的输入。UnitTest 提供了一个基类“TestCase”用来创建新的 TestCases。
（3）Test Suite 可以简单理解为 Test Case 的集合，主要用于对于集成管理要在一起执行的测试用例。
（4）Test Runner 也是 UnitTest 的一个重要组件，主要用于协调测试的执行并提供结果输出给用户参考。

四个概念 | 说明
--- | ----
Test Case | 测试用例，是最小的测试单元，用于检查特定输入集合的特定返回值。<br>TestCase 基类，创建的测试类需要继承该基类，可用来创建新的测试用例。
Test Suite | 测试套件，用于组装一组要运行的测试用例。<br>TestSuite 类来创建测试套件。
Test Runner | 测试执行并提供结果。可以使用图形、文本界面或返回特殊值来展示结果。<br>TextTestRunner 类运行测试用例，可用 HTMLTestRunner 运行类生成 HTML 报告。
Test Fixture | 代表执行一个或多个测试所需的环境准备，以及关联的清理动作。例如，创建临时或代理数据库、目录，或启动服务器进程。<br>setUp()/tearDown()、setUpClass()/tearDownClass()、setUpModule()/tearDownModule()来设置各级初始化和还原。

setUpModule/tearDownModule：在整个模块的开始与结束时被执行。
setUpClass/tearDownClass：在测试类的开始与结束时被执行。
setUp/tearDown：在测试用例的开始与结束时被执行。

（1）所有的测试用例类要继承基本类 unittest.TestCase。
（2）unittest.main()的作用是使一个单元测试模块变为可直接运行的测试脚本。
main()方法使用 TestLoader 类来搜索所有包含在该模块中以“test”命名开头的测试方法，并自动执行他们。执行方法的默认顺序是，根据 ASCII 码的顺序加载测试用例，数字与字母的顺序为 0-9、A-Z、a-z。因此 A 开头的方法会优先执行，a 开头的方法会后执行。

需要注意的是，setUpClass/tearDownClass 为类方法，需要通过@classmethod 进行装饰。另外，方法的参数为 cls。其实，cls 与 self 并没有什么本质区别，都只表示方法的第一个参数。

```PYTHON
Python 3.9.1 (tags/v3.9.1:1e5d33e, Dec  7 2020, 17:08:21) [MSC v.1927 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license()" for more information.
>>> import unittest
>>> def setUpModule():
    	print("==== module init ====")

>>> def tearDownModule():
    	print("==== module end ====")

	
>>> class TestAdd(unittest.TestCase):
    	@classmethod
    	def setUpClass(cls):
    		print("== 类初始化 ==")
    	@classmethod
    	def tearDownClass(cls):
    		print("== 类还原 ==")
    	def setUp(self):
    		print("== 用例初始化 ==")
    	def tearDown(self):
    		print("== 用例还原 ==")
    	def test_add_001(self):
    		self.assertEqual(1 + 2, 12)
    	def test_add_002(self):
    		self.assertEqual(1 + 2, 3)
    	def test_add_003(self):
    		self.assertEqual(1 - 2, -1)
    	def test_add_004(self):
    		self.assertEqual(1 * 2, 2)
    	def test_add_005(self):
    		self.assertEqual(1 / 2, 0.5)

    		
>>> if __name__ == "__main__":
    	unittest.main()

	
==== module init ====
== 类初始化 ==
== 用例初始化 ==
== 用例还原 ==
F== 用例初始化 ==
== 用例还原 ==
.== 用例初始化 ==
== 用例还原 ==
.== 用例初始化 ==
== 用例还原 ==
.== 用例初始化 ==
== 用例还原 ==
.== 类还原 ==
==== module end ====

======================================================================
FAIL: test_add_001 (__main__.TestAdd)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "<pyshell#8>", line 13, in test_add_001
AssertionError: 3 != 12

----------------------------------------------------------------------
Ran 5 tests in 0.340s

FAILED (failures=1)
>>> if __name__ == "__main__":
    	suite = unittest.TestSuite()
    	suite.addTest(TestAdd("test_add_002"))
    	suite.addTest(TestAdd("test_add_001"))
    	suite.addTest(TestAdd("test_add_003"))
    	suite.addTest(TestAdd("test_add_004"))
    	suite.addTest(TestAdd("test_add_005"))
    	unittest.TextTestRunner().run(suite)

    	
==== module init ====
== 类初始化 ==
== 用例初始化 ==
== 用例还原 ==
.== 用例初始化 ==
== 用例还原 ==
F== 用例初始化 ==
== 用例还原 ==
.== 用例初始化 ==
== 用例还原 ==
.== 用例初始化 ==
== 用例还原 ==
.== 类还原 ==
==== module end ====

======================================================================
FAIL: test_add_001 (__main__.TestAdd)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "<pyshell#8>", line 13, in test_add_001
AssertionError: 3 != 12

----------------------------------------------------------------------
Ran 5 tests in 0.289s

FAILED (failures=1)
<unittest.runner.TextTestResult run=5 errors=0 failures=1>
>>> if __name__ == "__main__":
    	suite = unittest.TestSuite()
    	suite.addTest(TestAdd("test_add_002"))
    	suite.addTest(TestAdd("test_add_001"))
    	suite.addTest(TestAdd("test_add_003"))
    	suite.addTest(TestAdd("test_add_004"))
    	suite.addTest(TestAdd("test_add_005"))
    	unittest.TextTestRunner(verbosity=2).run(suite)

    	
==== module init ====
== 类初始化 ==
test_add_002 (__main__.TestAdd) ... == 用例初始化 ==
== 用例还原 ==
ok
test_add_001 (__main__.TestAdd) ... == 用例初始化 ==
== 用例还原 ==
FAIL
test_add_003 (__main__.TestAdd) ... == 用例初始化 ==
== 用例还原 ==
ok
test_add_004 (__main__.TestAdd) ... == 用例初始化 ==
== 用例还原 ==
ok
test_add_005 (__main__.TestAdd) ... == 用例初始化 ==
== 用例还原 ==
ok
== 类还原 ==
==== module end ====

======================================================================
FAIL: test_add_001 (__main__.TestAdd)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "<pyshell#8>", line 13, in test_add_001
AssertionError: 3 != 12

----------------------------------------------------------------------
Ran 5 tests in 0.382s

FAILED (failures=1)
<unittest.runner.TextTestResult run=5 errors=0 failures=1>
```

## 断言
在执行测试用例的过程中，最终测试用例执行成功与否，是通过测试得到的实际结果与预期结果进行比较得到的。
断言就是判断实际测试结果与预期结果是否一致，一致则测试通过，否则失败。
unittest 框架的 TestCase 类提供的用于测试结果的断言方法如表所示。

方法 | 检查 | 说明
---- | ---- | ----
assertEqual(a, b) | a == b | 两个值相等不仅仅指内容相同而且还要类型相同
assertNotEqual(a, b) | a != b | 对返回的测试结果执行布尔类型的校验
assertTrue(x) | bool(x) is True | 一个值是否包含在另外一个值的范围内
assertFalse(x) | bool(x) is False
assertIs(a, b) | a is b
assertIsNot(a, b) | a is not b
assertIsNone(x) | x is None
assertIsNotNone(x) | x is not None
assertIn(a, b) | a in b
assertNotIn(a, b) | a not in b
assertIsInstance(a, b) | isinstance(a, b)
assertNotIsInstance(a, b) | not isinstance(a, b) 

## 套件：组织用例并执行
使用套件组织用例并执行
suites = unittest.TestSuite()
suites = unittest.defaultTestLoader.discover("./test_case",  'test*.py')
组织用例的四种方式：添加单个用例，添加测试类，添加模块，添加目录下的匹配模块。

如何执行多个测试文件呢？
unittest中的TestLoader类提供的discover()方法可以从多个文件中查找测试用例。该类根据各种标准加载测试用例，并将它们返回给测试套件。
一般不需要创建这个类的实例。unittest 提供了可以共享的 defaultTestLoader 类，可以使用其子类或方法创建实例，discover()方法就是其中之一。
```PYTHON
import unittest  
from os.path import dirname, abspath  
import sys  
sys.path.append(dirname(abspath(__file__)) + "\\test_case")  
from test_leapyear import TestLeapYear, TestLeapYearParas  
import test_calculator  
  
  
class Suites(unittest.TestSuite):  
    suites = unittest.TestSuite()  
    # 添加用例到组件的四种方式  
  
    # 添加单个用例    suites.addTest(TestLeapYear("test_100_year"))  
    suites.addTest(TestLeapYear("test_no_100_year"))  
  
    loader = unittest.TestLoader()  
    # 添加类（场景：一个接口定义一个测试用例类）  
    suites.addTest(loader.loadTestsFromTestCase(TestLeapYearParas))  
  
    # 添加模块（场景：一个模块有多个接口，如注册登录）  
    suites.addTest(loader.loadTestsFromModule(test_calculator))  
  
    # 添加路径下的所有用例（场景：一个项目下的所有测试用例）  
    # discover()方法会自动根据测试用例目录查找匹配的测试用例文件，并将找到的测试用例添加到测试套件中。    
    suites = unittest.defaultTestLoader.discover("./test_case",  'test*.py')  
  
    # unittest.TextTestRunner().run(suites)  
    unittest.TextTestRunner(verbosity=2).run(suites)
```

测试执行时，使用 Test Runner 代替 unittest.main()，有两个优点。
一是可以控制用例的执行顺序。测试用例的执行顺序可以由测试套件的添加顺序控制（调用 TestSuite 类下面的 addTest() 来添加测试用例），而 main()方法只能按照测试类、方法的名称来执行测试用例。例如，TestA 类比 TestB 类先执行，test_add()用例比test_div()用例先执行。
二是可以灵活地控制要执行的测试用例。当一个测试文件中有很多测试用例时，并不是每次都要执行所有的测试用例，尤其是比较耗时的 UI 自动化测试。

### 多级目录查找用例：使用包
当测试用例的数量达到一定量级时，就要考虑目录划分，比如规划如下多级测试目录。

>test_project
>├──/test_case/
>│ ├── test_bbb/
>│ │ ├── test_ccc/
>│ │ │ └── test_c.py
>│ │ └── test_b.py
>│ ├── test_ddd/
>│ │ └── test_d.py
>│ └── test_a.py
>└─ run_tests.py

对于上面的目录结构，如果将 discover()方法中的 start_dir 参数定义为“./test_case”目录，那么只能加载 test_a.py 文件中的测试用例。
如何让 unittest 查找 test_case/下子目录中的测试文件呢？
方法很简单，就是在每个子目录下放一个\_\_init\_\_.py 文件。该文件的作用是将一个目录标记成一个标准的 Python 模块，即包。

### 用例执行顺序
1，通过命名控制执行顺序
unittest默认根据ASCII码的顺序加载测试用例的（数字与字母的顺序为0~9，A~Z，a~z），所以 TestAdd 类会优先于 TestBdd 类被执行，est_aaa()方法会优先于 test_ccc()方法被执行，也就是说，它并不是按照测试用例的创建顺序从上到下执行的。

discover()方法和 main()方法的执行顺序是一样的。对于测试目录与测试文件来说，上面的规律同样适用。test_aaa.py 文件会优先于 test_bbb.py 文件被执行。所以，如果想让某个测试文件先执行，可以在命名上加以控制。

2，通过套件 unittest.TestSuit().addTest() 加载用例，控制执行顺序。
声明测试套件 TestSuite 类，通过 addTest()方法按照一定的顺序来加载测试用例，执行顺序与 addTest()方法加载测试用例的顺序相同。不过，当测试用例非常多时，不推荐用这种方法创建测试套件。

### 跳过测试和预期失败
对测试用例方法或测试用例类，可以使用修饰器，来跳过测试，或预期失败。

修饰器 | 说明
---- | ----
@unittest.skip(reason) | 无条件地跳过装饰的测试，需要说明跳过测试的原因。
@unittest.skipif(condition, reason) | 如果条件为真，则跳过装饰的测试。
@unittest.skipUnless(condition, reason) | 当条件为真时，执行装饰的测试。
@unittest.expectedFailure | 不管执行结果是否失败，都将测试标记为失败，但不会抛出失败信息。
```PYTHON
import unittest


class MyTest(unittest.TestCase):
    @unittest.skip("直接跳过测试")
    def test_skip(self):
        print("test aaa")

    @unittest.skipIf(3 > 2, "当条件为真时跳过测试")
    def test_skip_if(self):
        print('test bbb')

    @unittest.skipUnless(3 > 2, "当条件为真时执行测试")
    def test_skip_unless(self):
        print('test ccc')

    @unittest.expectedFailure
    def test_expected_failure(self):
        self.assertEqual(2, 3)


if __name__ == '__main__':
    unittest.main()


# 执行结果
test ccc
xss.
----------------------------------------------------------------------
Ran 4 tests in 0.001s

OK (skipped=2, expected failures=1)
```

## 测试固件分离
在 UI 自动化测试中，不管编写哪个模块的测试用例，都需要首先在测试类中编写测试固件初始化 WebDriver 类及打开浏览器，测试用例执行完成后还需要关闭浏览器，这部分的代码如下：
```PYTHON
def setUp(self):
    self.driver=webdriver.Firefox()
    self.driver.maximize_window()
    self.driver.get('http://www.baidu.com')
    self.driver.implicitly_wait(30)

def tearDown(self):
    self.driver.quit()
```

在每一个测试类中都要编写以上代码，因此需要重复编写很多代码。是否可以把测试固件这部分代码分离出去，测试类直接继承分离出去的类呢？我们把测试固件分离到 init.py模块中，类名称为 InitTest，代码如下：
```PYTHON
#!/usr/bin/env python
#-*-coding:utf-8-*-

import unittest
from selenium import webdriver

class InitTest(unittest.TestCase):

    def setUp(self):
        self.driver = webdriver.Firefox()
        self.driver.maximize_window()
        self.driver.get('http://www.baidu.com')
        self.driver.implicitly_wait(30)
    
    def tearDown(self):
        self.driver.quit()
```

测试类继承了 InitTest，继承后，在测试类中直接编写要执行的测试用例，代码如下：
```PYTHON
#!/usr/bin/env python
#-*-coding:utf-8-*-

import unittest
from init import InitTest

class BaiduTest(InitTest):
    def test_001(self):
        pass
    
    def test_002(self):
        pass

if __name__ == '__main__':
    unittest.main(verbosity=2)
```
首先需要导入 init 模块中的 InitTest 类，测试类 BaiduTest 继承InitTest类。这样执行测试类后，会先执行 setUp方法，再执行具体的测试用例，最后执行 tearDown 方法。python 的类继承的方式解决了在每个测试类中都需要编写测试固件的问题。把测试固件分离出去后，即使后期测试地址发生变化，只需要修改 init模块中 InitTest类中的 url地址即可，而不需要在每个测试类修改测试地址，减少了编写重复性代码的开销。

## 测试报告
HTMLTestRunner 是 unittest 的一个扩展，它可以生成易于使用的 HTML 测试报告。HTMLTestRunner 是在 BSD 许可证下发布的。
下载地址：http://tungwaiyip.info/software/HTMLTestRunner.html。

因为该扩展不支持 Python 3，所以需要做一些修改，使它可以在 Python 3 下运行。另外，还做了一些样式调整，使其看上去更加美观。
GitHub 地址：https://github.com/defnngj/HTMLTestRunner。

HTMLTestRunner 是一个独立的 HTMLTestRunner.py 文件，既可以把它当作 Python 的第三方库来使用，也可以将把它当作项目的一部分来使用。
如果把 HTMLTestRunner 当作项目的一部分来使用，就把它放到项目目录中。推荐这种方式，因为可以方便地定制生成的 HTMLTestRunner 报告。
如果把它作为第三方库，就把它单独放到 Python 的安装目录下面，如 C:\\Python37\\Lib\\。

HTMLTestRunner.HTMLTestRunner() 参数
- stream：指定生成 HTML 测试报告的文件，必填。
- verbosity：指定日志的级别，默认为 1。如果想得到更详细的日志，则可以将参数修改为 2。
- title：指定测试用例的标题，默认为 None。
- description：指定测试用例的描述，默认为 None。

生成测试报告
```PYTHON
import unittest  
import time  
from HTMLTestRunner import HTMLTestRunner

class Suites(unittest.TestSuite):
    suites = unittest.defaultTestLoader.discover("./test_case",  'test*.py')
    
    # HTML 测试报告  
    report_html = "report/report_{0}.html".format(time.strftime("%Y-%m-%d %H_%M_%S"))  
    with open(report_html, "w", encoding="utf-8") as fp:  
        HTMLTestRunner(  
            stream=fp,  
            title='测试报告',  
            description='运行环境：Windows 10, Chrome 浏览器'  
        ).run(suites)
```

**测试报告中用例的注释**
在编写功能测试用例时，每条测试用例都有标题或说明，那么能否为自动化测试用例加上中文的标题或说明呢？答案是肯定的，可以利用注释实现。

Python 的注释有两种，一种叫作 comment，另一种叫作 doc string。前者为普通注释，后者用于描述函数、类和方法 。在类或方法的下方，可以通过三引号（""" """ 或 ''' '''）添加 doc string 类型的注释。这类注释在平时调用时不会显示，只有通过 help()方法查看时才会被显示出来。

因为 HTMLTestRunner 可以读取 doc string 类型的注释，所以，我们只需给测试类或方法添加这种类型的注释即可。
```PYTHON
class XXX():
    """ 注释 """
    pass

    def xxx(self):
        """ 注释 """
        pass
```

**测试报告命名**
因为测试报告的名称是固定的，所以每次新的测试报告都会覆盖上一次的。如果不想被覆盖，那么只能每次在运行前都手动修改报告的名称。这样显然非常麻烦，我们最好能为测试报告自动取不同的名称，并且还要有一定的含义。时间是个不错的选择，因为它可以标识每个报告的运行时间，更主要的是，时间永远不会重复。

在 Python 的 time 模块中提供了各种关于时间操作的方法，利用这些方法可以完成这个需求。
- time.time()：获取当前时间戳。
- time.ctime()： 当前时间的字符串形式。
- time.localtime()：当前时间的 struct_time 形式。
- time.strftime()：用来获取当前时间，可以将时间格式化为字符串。

可以通过 time.strftime() 方法以指定的格式获取当前日期时间，再通过字符串格式化给测试报告文件命名。
```PYTHON
report_html = "report/report_{0}.html".format(time.strftime("%Y-%m-%d %H_%M_%S"))

>>> time.strftime("%Y-%m-%d %H_%M_%S")
'2022-08-17 10_57_21'
```

## 数据驱动_参数化
### 读取数据文件
大多数文章和资料都把“读取数据文件”看作数据驱动的标志。
在 unittest 中，可以使用读取数据文件来实现参数化。如 txt、CSV、excel，详细请参考 [[文件处理]]。

### Parameterized
Parameterized 是 Python 的一个参数化库，同时支持 unittest、Nose 和 pytest 单元测试框架。支持 pip 安装。
GitHub 地址：https://github.com/wolever/parameterized。

```cmd
pip install  parameterized
```

使用步骤：
1，导入 Parameterized 库下面的 parameterized 类。
2，使用 @parameterized.expand() 装饰用例方法，数据采用内嵌元组的列表，元组元素分别对应用例方法中的参数。每个元组都可以被认为是一条测试用例。在测试用例中，通过参数来取每个元组中的数据。

```PYTHON
import unittest
from parameterized import parameterized

class LeapYear(object):  
    def __init__(self, year):  
        self.year = year  
  
    def is_leap_year(self):  
        return self.year % 100 != 0 and (self.year % 4 == 0) or (self.year % 400 == 0)

class TestLeapYearParas(unittest.TestCase):  
    @parameterized.expand([  
        (-400, True), (0, True), (400, True), (2000, True), (10000, True),  
        (-100, False), (100, False), (300, False), (500, False), (1900, False), (2100, False),  
    ])  
    def test_100_year_paras(self, year, expected):  
        """ 参数化，测试能被100整除的年份 """  
        self.assertEqual(LeapYear(year).is_leap_year(), expected)

# 结合 csv 使用
class TestLeapYearCSVParas(unittest.TestCase):  
    # 读取 csv 数据  
    data = csv.reader(codecs.open("../test_data/testdata_leapyear.csv", "r", "utf-8"))  
    @parameterized.expand([tuple(map(eval, line)) for line in islice(data, 1, None)])  
    def test_100_year_csv(self, year, expected):  
        """ csv 参数化，测试能被100整除的年份 """  
        self.assertEqual(LeapYear(year).is_leap_year(), expected)
```

### DDT
DDT（Data-Driven Tests）是针对 unittest 单元测试框架设计的扩展库。允许使用不同的测试数据来运行一个测试用例，并将其展示为多个测试用例。支持 pip 安装。
GitHub 地址：https://github.com/datadriventests/ddt。

```CMD
pip install ddt
pip install PyYAML
```

使用步骤
1， 导库 
2，使用 @ddt 装饰器装饰测试类。
3，使用四种方式的参数化：列表、元组、字典、文件（json、yaml）。

在 ddt 模块中，@data 表示元组的列表数据，@unpack 表示用来解压元组到多个参数。ddt 库应用在 UI 自动化测试中，实现编写一条测试用例的代码验证多个测试点。

DDT 同样支持数据文件的参数化。它封装了数据文件的读取，让我们更专注于数据文件中的内容，以及在测试用例中的使用，而不需要关心数据文件是如何被读取进来的。

需要注意的是
使用字典时，字典的 key 与测试方法的参数要保持一致。
使用 yaml 文件时，需要先 pip 安装 PyYAML 库。取值与 JSON 文件不同，因为每一条用例都被解析为一个内嵌字典的列表，如`[{'year': 1988}, {'expected': True}]`，所以要想取到对应的值，则需要通过索引和key值`case[0]["year"]), case[1]["expected"]`的方式获取。

```PYTHON
from ddt import ddt, data, unpack, file_data


@ddt  
class TestLeapYearDDT(unittest.TestCase):  
    """ DDT 测试 参数化四种方式：列表、元组、字典、文件（json、yaml）"""  
    # @data([1988, True], [1992, True], [2021, True], [2024, True])  
    # @data((1988, True), (1992, True), (2020, True), (2024, True))    
    # @data ({"year": 1988, "expected": True}, {"year": 1992, "expected": True},    
    #        {"year": 2020, "expected": True}, {"year": 2022, "expected": True}, )    
    @file_data("../test_data/ddt_data.json")  
    @unpack  
    def test_no_100_year_ddt_json(self, year, expected):  
        """ DDT（列表、元组、字典、json） 测试，测试不能被100整除的年份 """  
        self.assertEqual(LeapYear(year).is_leap_year(), expected)  
  
    @file_data("../test_data/ddt_data.yaml")  
    @unpack  
    def test_no_100_year_ddt_yaml(self, case):  
        """ DDT（yaml） 测试，测试不能被100整除的年份 """  
        self.assertEqual(LeapYear(case[0]["year"]).is_leap_year(), case[1]["expected"])



# 数据文件准备
# ddt_data.jso
{  
  "1988 True":{"year": 1988, "expected": true},  
  "1992 True":{"year": 1992, "expected": true},  
  "2020 True":{"year": 2020, "expected": true},  
  "2024 True":{"year": 2024, "expected": true}  
}

# ddt_data.yaml
1988 True:  
  - year: 1988  
  - expected: True  
1992 True:  
  - year: 1992  
  - expected: True  
2020 True:  
  - year: 2020  
  - expected: True  
2022 True:  
  - year: 2022  
  - expected: True  
2024 True:  
  - year: 2024  
  - expected: True
```

## 邮件通知
发件邮箱（网易163）需要开启POP3/SMTP服务授权码：QTCBUTVNEXBAGWRI。可作为使用第三方工具登录的密码，如使用 Python 登录。

1，使用标准库 [[smtplib]] 发送邮件
2，使用 [[yagmail]] 发送邮件，非常简单，推荐

yagmail 组合到自动化测试中，发送测试报告。编写 send_mail.py 文件，方便调用。

```PYTHON
import yagmail  
import time  
  

# 将测试报告作为附件发送到指定邮箱  
# 不能以正文原因：HTMLTestRunner 报告在展示时引用了 Bootstrap 样式库，当作为正文“写死”在邮件中时，会导致样式丢失。
def send_mail(report_html):  
    """ 邮件发送测试报告 """  
    yag = yagmail.SMTP("yixianjun8052@163.com", "QTCBUTVNEXBAGWRI", "smtp.163.com")  
    subject = "主题，Python 自动化测试报告 {0}".format(time.strftime("%Y-%m-%d %H:%M:%S"))  
    contents = ['正文，请看附件。']  
    yag.send('yixianjun8052@163.com', subject, contents, report_html)  
    print('email has send out !')
```

套件调用
```PYTHON
import unittest
import time
from HTMLTestRunner import HTMLTestRunner
from send_mail import send_mail


class Suites(unittest.TestSuite):   
    suites = unittest.defaultTestLoader.discover("./test_case",  'test*.py')  
  
    # HTML 测试报告    
    report_html = "test_report/report_{0}.html".format(time.strftime("%Y-%m-%d %H_%M_%S"))  
    with open(report_html, "w", encoding="utf-8") as fp:  
        HTMLTestRunner(  
            stream=fp,  
            title='测试报告',  
            description='运行环境：Windows 10, Chrome 浏览器'  
        ).run(suites)  

    # 发送邮件
    send_mail(report_html)
```

整个程序的执行过程可以分为两部分：
（1）定义测试报告文件，并赋值给变量 html_report，通过 HTMLTestRunner 运行测试用例，将结果写入文件后关闭。
（2）调用 send_mail()函数，并传入 html_report 文件。在 send_mail()函数中，把测试报告作为邮件的附件发送到指定邮箱。

为什么不把测试报告的内容读取出来作为邮件正文发送呢？因为 HTMLTestRunner 报告在展示时引用了 Bootstrap 样式库，当作为邮件正文“写死”在邮件中时，会导致样式丢失，所以作为附件发送更为合适。

# 示例：web登录
案例：iwebshop 登录测试，以用户名为例

| 框架结构          | 说明                                         | 关系                      |
| ----------------- | -------------------------------------------- | ------------------------- |
| commonlib.py      | 通用基础功能：浏览器操作，元素定位和操作等。 | 继承 object 类            |
| funcs.py          | 测试点功能函数：登录的实现步骤。             | 继承 Commonlib 类         |
| testcase.py       | 用例库                                       | 继承 unittest.TestCase 类 |
| testsuite.py      | 用例套件：自选用例批量执行                   |                           |
| HTMLTestRunner.py | 输出 html 测试报告。下载时注意版本适配。     | 网络第三方扩展            |

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
from bysms_test.unittest.commonlib import Commonlib  
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

testcase.py

```python
import unittest  
from bysms_test.unittest.funcs import Funcs  
  
  
class TestCase(unittest.TestCase):
    
    func = Funcs()  
    
    def setUp(self) -> None:  
        """  
        每个用例执行前的动作
        没有可不写
        :return:  
        """
        pass  
    
    def tearDown(self) -> None:  
        """  
        每个用例执行后的动作：删除浏览器 cookie
        没有可不写
        :return:  
        """
        self.func.alert_accept()  
        self.func.del_cookies()  
        if self.func.get_url() == "http://127.0.0.1/mgr/sign.html":  
            self.func.clear('css selector', '#username')  
            self.func.clear('css selector', '#password')  
  
    def test_login_001(self):  
        """ 正确的数据 """  
        self.assertEqual(self.func.login('byhy', '88888888'), '管理员')  
  
    def test_login_002(self):  
        """ 用户名错误：为空 """  
        self.assertEqual(self.func.login('', '88888888'), "请输入用户名")  
  
    def test_login_003(self):  
        """ 用户名错误：为空格 """  
        self.assertEqual(self.func.login(' ', '88888888'), '登录失败 : 用户名或者密码错误')  
  
    def test_login_004(self):  
        """ 用户名错误：前后有空格 """  
        self.assertEqual(self.func.login(' byhy ', '88888888'), '登录失败 : 用户名或者密码错误')  
  
    def test_login_005(self):  
        """ 用户名错误：少一位 """  
        self.assertEqual(self.func.login('byh', '88888888'), '登录失败 : 用户名或者密码错误')  
  
    def test_login_006(self):  
        """ 用户名错误：多一位 """  
        self.assertEqual(self.func.login('byhyy', '88888888'), '登录失败 : 用户名或者密码错误')  
  
    def test_login_007(self):  
        """ 密码错误：为空 """  
        self.assertEqual(self.func.login('byhy', ''), '请输入密码')  
  
    def test_login_008(self):  
        """ 密码错误：为空格 """  
        self.assertEqual(self.func.login('byhy', ' '), '登录失败 : 用户名或者密码错误')  
  
    def test_login_009(self):  
        """ 密码错误：位数少一位 """  
        self.assertEqual(self.func.login('byhy', '8888888'), '登录失败 : 用户名或者密码错误')  
  
    def test_login_010(self):  
        """ 密码错误：组成错误 """  
        self.assertEqual(self.func.login('byhy', '12345678'), '登录失败 : 用户名或者密码错误')  
  
    def test_login_011(self):  
        """ 密码错误：首尾空格 """  
        self.assertEqual(self.func.login('byhy', '  88888888 '), '登录失败 : 用户名或者密码错误')  
  
  
if __name__ == '__main__':  
    unittest.main()
```

testsuite.py

```python
import unittest  
from bysms_test.unittest import testcase  
from bysms_test.unittest.HTMLTestRunner import HTMLTestRunner  
import time  
  
  
class TestCaseSuite(object):  
    # 实例化测试套件，并将用例添加到对象中  
    # cases = ["test_login_001", "test_login_002", "test_login_003"]
    cases = ["test_login_%03d" % (i+1) for i in range(11)]
    case_suite = unittest.TestSuite()  
    for case in cases:  
        case_suite.addTest(testcase.TestCase(case))  
  
    # 执行测试套件，并控制台输出测试结果  
    # unittest.TextTestRunner(verbosity=2).run(case_suite)
    
    # html 网页版报告
    report_html = "report/report_%s.html" % (time.strftime("%Y%m%d_%H%M%S", time.gmtime()))  
    with open(report_html, "w", encoding="utf8") as f:  
        HTMLTestRunner(stream=f, verbosity=2).run(case_suite)  
  

if __name__ == '__main__':  
    unittest.main()
```
