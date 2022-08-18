# **smtplib模块**

smtplib模块，作用==发送邮件==

smtplib使用较为简单。以下是最基本的语法。

导入及使用方法如下：

```python
import smtplib
smtp = smtplib.SMTP()  # 实例化SMTP()
# 连接邮箱服务器，host:指定连接的邮箱服务器，port：指定连接服务器的端口号，默认为25. 默认很可能会失败，端口号具体内容需要查询邮件服务提供商
smtp.connect('smtp.qq.com,465') 
# 登录邮箱，登录邮箱的用户名，登录邮箱的密码（授权码）
smtp.login(username, password) 
# 发送邮件，from_addr:邮件发送者地址，to_addrs:邮件接收者地址。字符串列表['接收地址1','接收地址2','接收地址3',...]或'接收地址'，msg：发送消息：邮件内容。
smtp.sendmail(send_add, to_add, msg.as_string()) 
# 用于结束SMTP会话
smtp.quit()
```