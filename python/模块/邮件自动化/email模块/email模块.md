# **email模块**

email模块主要负责构造邮件：指的是邮箱页面显示的一些构造，如发件人，收件人，主题，正文，附件等。

email模块下有mime包，mime英文全称为“Multipurpose Internet Mail Extensions”，即多用途互联网邮件扩展，是目前互联网电子邮件普遍遵循的邮件技术规范。

email模块，作用==设置邮件==

导入及使用方法如下：

```
from email.mime.text import MIMEText
from email.header import Header
# 第三方 SMTP 服务
mail_host = "smtp.qq.com"  # 设置服务器
mail_user = "******@qq.com"  # 用户名
mail_pass = "famaiqllddfsbjag"  # 授权码
sender = '******@qq.com' # 发件箱
receivers = ['****@qq.com']  # 接收邮件，可设置为你的QQ邮箱或者其他邮箱

message = MIMEText('Python 邮件发送测试...', 'plain', 'utf-8')
message['From'] = Header("Python SMTP教程", 'utf-8') #括号里的对应发件人邮箱昵称（随便起）、发件人邮箱账号
message['To'] = Header("测试", 'utf-8') #括号里的对应收件人邮箱昵称、收件人邮箱账号
subject = 'Python SMTP 邮件测试' #主题
message['Subject'] = Header(subject, 'utf-8')
```



