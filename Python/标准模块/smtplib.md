使用标准库 smtplib 发送邮件步骤

```PYTHON
>>> import smtplib
>>> from email.mime.text import MIMEText
>>> from email.header import Header
>>> 
>>> subject = 'Python email test'
>>> msg = MIMEText('<html><h1>你好！</h1></html>', 'html', 'utf-8')
>>> msg['Subject'] = Header(subject, 'utf-8')
>>> 
>>> smtp = smtplib.SMTP()
>>> 
>>> smtp.connect("smtp.163.com")
(220, b'163.com Anti-spam GT for Coremail System (163com[20141201])')
>>> smtp.ehlo()
(250, b'mail\nPIPELINING\nAUTH LOGIN PLAIN XOAUTH2\nAUTH=LOGIN PLAIN XOAUTH2\ncoremail 1Uxr2xKj7kG0xkI17xGrU7I0s8FY2U3Uj8Cz28x1UUUUU7Ic2I0Y2Urbqv38UCa0xDrUUUUj\nSTARTTLS\nID\n8BITMIME')
>>> smtp.helo()
(250, b'OK')
>>> smtp.login("yixianjun8052@163.com", "QTCBUTVNEXBAGWRI")
(235, b'Authentication successful')
>>> smtp.sendmail("yixianjun8052@163.com", "yixianjun8052@126.com", msg.as_string())
{}
>>> smtp.quit()
(221, b'Bye')


import smtplib  
from email.mime.text import MIMEText  
from email.mime.multipart import MIMEMultipart  
from email.header import Header  
  
  
# 设置 smtplib 的参数  
smtpserver = "smtp.163.com"  
username = "yixianjun8052@163.com"  
password = "QTCBUTVNEXBAGWRI"  
sender = "yixianjun8052@163.com"  
receiver = "yixianjun8052@163.com"  
# 当收件人为多个时，使用列表  
# receivers = ["xxx@163.com", "xxx@qq.com"]  
  
subject = '测试 Python 邮件发送测试报告'  
  
# 构造邮件对象 MIMEMultipart 对象  
# 指定发件人、发件人、主题，组合正文和附件  
msg = MIMEMultipart()  
msg['Subject'] = Header(subject, 'utf-8')  
msg['From'] = sender  
msg['To'] = receiver  
# 当有多个收件人时，通过 join 将列表转换为分号“;”为间隔的字符串  
# msg['To'] = ";".join(receivers)  
  
# 构造文字内容  
# 注意：由于邮件客户端首先渲染最后的子部分，所以必须在替代的纯文本(text, part1)之后添加html(part2)。  
# 如果在替代的纯文本之前添加html，会导致替代的纯文本会变成附件。  
text = """ hi,  
刘亦菲  
赶紧出来拍戏。"""  
# 将 text 转换为 plain 的 MIMEText 对象  
text_plain = MIMEText(text, 'plain', 'utf-8')  
msg.attach(text_plain)  
  
# 构造 html 内容  
html = """ \  
<html>  
    <body>
        <p>hi,<br>            
            刘亦菲<br>            
            赶紧出来拍戏。        
        </p>    
    </body>
</html>"""  
  
# 将 html 转换为 html 的 MIMEText 对象  
text_html = MIMEText(html, 'html', 'utf-8')  
msg.attach(text_html)  
  
# 构造附件：内容、格式、编码、类型、文件名  
with open('report.html', 'rb') as f:  
    send_att = f.read()  
att = MIMEText(send_att, 'base64', 'utf-8')  
att["Content-Type"] = 'application/octet-stream'  
att["Content-Disposition"] = 'attachment; filename="report.html"'  
msg.attach(att)  
  
# 登录邮箱，发送邮件  
smtp = smtplib.SMTP()  
smtp.connect(smtpserver)  
# set_debuglevel(1) 可以打印出和 SMTP 服务器交互的所有信息。  
# smtp.set_debuglevel(1)  
smtp.login(username, password)  
smtp.sendmail(sender, receiver, msg.as_string())  
smtp.quit()
```