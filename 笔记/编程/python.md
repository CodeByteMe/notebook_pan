# 使用python开启简单的HTTP服务
```bash
cd mydir
# 当前运行 不填写端口 默认8000
python -m SimpleHTTPServer 8888

# 当前终端后台运行
python -m SimpleHTTPServer 8888 &

# 完全后台运行
nohup python -m SimpleHTTPServer 8888 &
```
> SimpleHTTPServer有一个特性，如果待共享的目录下有`index.html`，那么`index.html`文件会被视为默认主页；如果不存在`index.html`文件，那么就会显示整个目录列表。  
> 在哪个目录下启动，就会以当前目录为根目录展示列表

[参考](https://www.cnblogs.com/jakaBlog/p/11394188.html)

# python使用smtp发送邮件
```python
# -*- coding: UTF-8 -*-

import smtplib
import sys
from email.mime.text import MIMEText
from email.header import Header

# SMTP
mail_host = "smtp.qq.com"
mail_user = "发送方的qq邮箱"
mail_pass = "smtp的授权码"

sender = '发送方的qq邮箱'
reveiver = ['接收方邮箱']
# 可以使用这种在文本中直接写死邮件内容的方式
# message = MIMEText('邮件内容', 'plain', 'utf-8')
# 也可以使用这种通过命令行传入邮件内容的方式
message = MIMEText(sys.argv[1], 'plain', 'utf-8')
message['From'] = Header('发送方名称<发送方邮箱>', 'utf-8')
message['To'] = Header('接收方名称', 'utf-8')

subject = '邮件标题'
message['Subject'] = Header(subject, 'utf-8')

try: 
    smtpObj = smtplib.SMTP()
    # 默认可以使用25端口, 
    # smtpObj.connect(mail_host, 25)
    # 如果telnet测试25端口不通的时候, 可以使用587端口
    smtpObj.connect(mail_host, 587)
    smtpObj.login(mail_user, mail_pass)
    smtpObj.sendmail(sender, reveiver, message.as_string())
except Exception as e: 
    print(e)
```
```bash
# 命令行加入参数, 脚本自动读取 我想发送一些内容 这个文本添加到邮件内容中
python3 sendMail.py 我想发送一些内容
# 若脚本已经写死, 可以直接这样发送
python3 sendMail.py
```
[参考](https://www.runoob.com/python/python-email.html)