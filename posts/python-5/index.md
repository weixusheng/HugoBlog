# Python发送邮件



使用第三方SMTP发送HTML邮件
<!--more-->

# 引入库

```python
import smtplib
from email.mime.text import MIMEText
from email.header import Header
```

# 设置smtp基本信息

```python
mail_host = "smtp.163.com"  # 设置服务器
mail_user = "cdt_cweme@163.com"  # 邮箱用户名
mail_pass = "xxx"  # 口令(需要在stmp设置页面上查看"
```

# 邮箱内容

```python
sender = 'cdt_cweme@163.com'	# 发送方
email_addrs = 'xxx@qq.com'	# 接收方
mail_msg = """
        <p>您的推送失败合同，法务正在处理中,请在一个小时后查看合同状态</p>
        <p><a href="https://docs.qq.com">可查看当前处理状态</a></p>
        """
message = MIMEText(mail_msg, 'html', 'utf-8') # 发送内容的格式
message['From'] = sender 	# 发送方
message['To'] = email_addrs # 接收方
subject = '合同推送法务失败-处理进度反馈' # 邮件主题
message['Subject'] = Header(subject, 'utf-8') # 主题文本格式
```

# 发送

```python
smtpObj = smtplib.SMTP()
smtpObj.connect(mail_host, 25)  # 25 为 SMTP 端口号
smtpObj.login(mail_user, mail_pass)
smtpObj.sendmail(sender, email_addrs, message.as_string())
```


