- 发送邮件(单发或群发)。
```
import smtplib
from email.mime.text import MIMEText

msg_from = 'c_******@163.com'
passward = '****'  # 授权码
msg_to = '72*****5@qq.com'
# 群发邮件
# msg_to = ['72******5@qq.com', '2********@qq.com']
subject = 'work-email'
content = '安贝儿程序可能出错了，请查看'

msg = MIMEText(content)
msg['Subject'] = subject
msg['From'] = msg_from
msg['To'] = msg_to
# 群发邮件
# msg['To'] = ','.join(msg_to)
s = smtplib.SMTP_SSL('smtp.163.com', 465)
try:
    s.login(msg_from, passward)
    s.sendmail(msg_from, msg_to, msg.as_string())
    print('发送成功')
except smtplib.SMTPException as e:
    print('发送失败' + format(e))
finally:
    s.quit()
```

- 通过yagmail发送邮件
```
import yagmail

yag = yagmail.SMTP(user='c_*****@163.com', password='授权码', host='smtp.163.com')
contents = ['this is body']
to_list = ['72*****5@qq.com', '2*****7@qq.com', '1*****9@qq.com']
yag.send(to_list, '主题：我的主题', contents)

```