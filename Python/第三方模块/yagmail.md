yagmail 是 Python 的一个第三方库，可以让我们以非常简单的方法实现自动发送邮件功能。通过 pip 命令安装。

GitHub 项目地址： https://github.com/kootenpv/yagmail。

```PYTHON
import yagmail  
  
# 连接邮箱服务器：发件箱 user、授权码 password、邮箱服务器 host
yag = yagmail.SMTP("yixianjun8052@163.com", "QTCBUTVNEXBAGWRI", "smtp.163.com")  
  
# 邮件正文：元素之间自动换行
contents = ['刘亦菲', '快出来拍戏']  
  
# 发送邮件：收件箱、邮件主题、邮件正文、附件路径  
yag.send("yixianjun8052@163.com", "subject: Test Python Send Email By Yagmail", contents)

# 给多个联系人发送邮件  
yag.send(['aa@126.com', 'bb@qq.com', 'cc@gmail.com'], 'subject', contents)

# 发送附件：只需指定本地附件的路径即可。  
yag.send('yixianjun8052@163.com',  
         'subject: Test Python Send Email With Attach By Yagmail', contents,  
         [".//report.html", ".//log.txt"])
```
