一般的开发语言都会有日志相关的模块（功能），比如 log4j、log4php 等功能强大，使用简单。Python 自身也提供了日志的标准库模块 logging。

logging 模块的日志级别设定有：
• DEBUG，通常打印的日志信息很详细，这种级别的设定场景一般是在进行问题定位和调试。
• INFO，信息详细程度仅次于 DEBUG，通常只记录关键的信息点，用于确认软件是否按照正常的预期在运行。
• WARNING，当某些异常信息发生时系统记录的日志信息，而此时软件一般是正常运行的。比如 App 服务器内存（Memory）抵达使用的临界点，比较成熟的软件会有日志提醒。
• ERROR，由于一个更加严重的问题导致软件运行不正常而记录的相关信息。比如内容溢出异常等。
• CRITICAL，当严重的错误发生时直接导致宕机、软件服务等无法使用，或者在访问时记录相关的信息。

日志的等级从低到高依次为 DEBUG < INFO < WARNING < ERROR < CRITICAL，但是相应的日志记录的信息量是逐步减少的。

logging 模块定义日志级别常用的函数：
• logging.debug(msg,\*args,\*\*kwargs)。
• logging.info(msg,\*args,\*\*kwargs)。
• logging.warning (msg,\*args,\*\*kwargs)。
• logging.error(msg,\*args,\*\*kwargs)。
• logging.critical(msg,\*args,\*\*kwargs)。

以上函数的作用是为了创建如 DEBUG、INFO、WARNING、ERROR、CRITICAL 等日志级别的日志。

此外，还有两种常用的函数，作用如下：
• logging.log(level,\*args,\*\*kwargs)：用于创建一个日志级别为 level 的日志记录。
• logging.basicConfig(\*\*kwargs)：对 root logger 进行配置，主要用于指定“日志级别”“日志格式”“日志输出位置/文件”“日志文件的打开模式”等信息。

logging 模块的四大组件如下：
（1）loggers（提供应用程序码直接使用的接口）。
（2）handlers（用于将日志记录发送到指定的位置）。
（3）filters（提供日志过滤功能）。
（4）formatters（提供日志输出格式设定功能）。

```PYTHON
import logging  
 
logging.debug("I am a debug level log.")  
logging.info("I am a info level log.")  
logging.warning("I am a warning level log.")  
logging.error("I am a error level log.")  
logging.critical(" I am a critical level log.")  
  
# logging.log(logging.DEBUG, "I am a debug level log.")  
# logging.log(logging.INFO, "I am a info level log.")  
# logging.log(logging.WARNING, "I am a warning level log.")  
# logging.log(logging.ERROR, "I am a error level log.")  
# logging.log(logging.CRITICAL, "I am a critical level log.")


# 控制台打印
D:\Programs\Python39\python.exe D:/workspace/pyproject/ui_auto/logging_test.py
WARNING:root:I am a warning level log.
ERROR:root:I am a error level log.
CRITICAL:root: I am a critical level log.

Process finished with exit code 0
```

我们会发现，DEBUG 和 NFO 级别的日志没有输出来。这是因为 logging 模块提供的日志记录函数所使用的日志器设置的级别为 WARNING，因此只有 WARNING 级别及大于该级别的（如 ERROR、CRITICAL）日志才会输出，而严重级别比 WARNNING 低的日志被丢弃了。

打印出来的日志信息如“WARNING:root:I am a warning level log.”各个字段的含义分别是日志级别、日志器名称和日志内容。日志之所以用这样的格式输出，是因为日志器中设置的是默认格式 BASIC_FORMAT，其值为“%(levelname)s:%(name)s:%(message)s”。

另外，为什么日志输出到控制台而没有输出到别的地方？原因是日志器中用的是默认输出位置“sys.stderr”。

如果要改变日志输出位置，需要手动调用函数 basicConfig()进行设置。basicConfig()函数的定义为“logging.basicConfig(\*\*kwargs)”。
该函数的参数描述如下：
• Filename：指定输出目标文件名，用于保存日志信息。设置该配置项后，日志就不会输出到控制台了。
• FileMode：指定日志文件的打开模式，默认为“a”，且仅在 filename 指定时生效。
• Format：指定输出的格式和内容，format 可以输出很多有用的信息。
• DateFmt：指定日期/时间格式。
• Level：指定日志器的日志级别。
• Stream：指定日志输出目标 stream，比如“sys.stdout”“sys.stderr”。需要注意的是，stream 配置项和 filename 配置项不能同时提供，可能会造成冲突和产生 ValueError异常。
• Style：Python3 之后新添加的配置项，用于指定 format 格式字符串的风格，可取值为“%”“{”和“$”。其默认值为“%”。

Handlers：Python 6.3 之后新添加的配置项。该选项如果被指定，它应该是一个创建了多个Handler 的可迭代对象，这些 handler 将会被添加到 root logger。需要说明的是，filename、stream 和 handlers 这三个配置项只能有一个存在，不能同时出现 2 个或 3 个，否则会引发 ValueError 异常。

logging 模块关于日志格式字符串字段的介绍如表 10.3 所示。

字段/属性名称 | 使用格式 | 描述
---- | ---- | ----
asctime | %(asctime)s | 日志事件发生的时间——可读时间，如：2019-01-07 16:49:45,896
created | %(created)f | 日志事件发生的时间——时间戳
relativeCreated | %(relativeCreated)d | 日志事件发生的时间相对于 logging 模块加载时间的相对毫秒数（目前还不知道用处）
msecs | %(msecs)d | 日志事件发生时间的毫秒部分
levelname | %(levelname)s | 该日志记录的文字形式的日志级别（'DEBUG', 'INFO', 'WARNING', 'ERROR','CRITICAL'）
levelno | %(levelno)s | 该日志记录的数字形式的日志级别（10, 20, 30, 40, 50）
name | %(name)s | 所使用的日志器名称，默认是'root'，因为默认使用的是 rootLogger
message | %(message)s | 日志记录的文本内容，通过 msg % args 计算得到的
pathname | %(pathname)s | 调用日志记录函数的源码文件的全路径
filename | %(filename)s | pathname 的文件名部分，包含文件后缀
module | %(module)s | filename 的名称部分，不包含后缀
lineno | %(lineno)d | 调用日志记录函数的源代码所在的行号
funcName | %(funcName)s | 调用日志记录函数的函数名
process | %(process)d | 进程 ID
processName | %(processName)s | 进程名称（Python 3 新增）
thread | %(thread)d | 线程 ID
threadName | %(thread)s | 线程名称

配置日志级别、日志输出、日志文件、日志格式、日期格式

```PYTHON
import logging  
  
# 配置日志级别，默认是 WARNING，也就是说默认情况下，只有WARNING级别以上的日志才打印。  
logging.basicConfig(level=logging.DEBUG,  
                    filename='log-se.log',  
                    filemode='a',  
                    format='%(asctime)s %(filename)s %(levelname)s %(message)s',  
                    datefmt="%m/%d/%Y %H:%M:%S %p")  
  
logging.debug("I am a debug level log.")  
logging.info("I am a info level log.")  
logging.warning("I am a warning level log.")  
logging.error("I am a error level log.")  
logging.critical(" I am a critical level log.")
```

代码执行完毕，在当前目录下生成一个日志文件“log-se.log”，内容如下：

>09/13/2022 22:35:04 PM logging_test.py DEBUG I am a debug level log.  
>09/13/2022 22:35:04 PM logging_test.py INFO I am a info level log.  
>09/13/2022 22:35:04 PM logging_test.py WARNING I am a warning level log.  
>09/13/2022 22:35:04 PM logging_test.py ERROR I am a error level log.  
>09/13/2022 22:35:04 PM logging_test.py CRITICAL  I am a critical level log.

日志应用实例

```PYTHON
# function.py 基础代码层
import logging

def log(str):
    logging.basicConfig(level=logging.INFO,
                        format='%(asctime)s %(filename)s %(levelname)s %(message)s',
                        datefmt='%a, %d %b %Y %H:%M:%S',
                        filename='log-selenium.log',
                        filemode='a')
    console = logging.StreamHandler()
    console.setLevel(logging.INFO)
    formatter = logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s')
    console.setFormatter(formatter)
    logging.getLogger('').addHandler(console)
    logging.info(str)


# 测试代码层
from function import log

#search_tickets("上海","杭州",1)
log("Read Excel Files to get test data.")
dic1 = read_excel("testdata.xlsx",0)
log("Begin to search tickets")
search_tickets(dic1[0][0],dic1[0][1],1)
log("End to search tickets")
log("Begin to get driver object.")
driver = return_driver()
```