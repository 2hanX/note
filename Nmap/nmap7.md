## 使用Nmap在服务器/客户端中查找漏洞

### **Heartbleed bug**

`nmap -script = ssl-heartbleed 192.168.0.114`

### **Poodle Bug**

`nmap -script ssl-poodle 192.168.0.114`

### **IRC Backdoor**

`nmap -sV –script=irc-unrealircd-backdoor  -p 6667 192.168.1.6`

### **MS08-67 Vulnerability**

`nmap  –script  smb-vuln-ms08-067  -p 445  192.168.0.114`

### **DOS Vulnerability**

`nmap -sV –script=rdp-ms12-020  -p 3389 192.168.0.114`

### **Vsftpd Backdoor**

`nmap –script ftp-vsftpd-backdoor  -p 21  192.168.1.6`

[原文](https://www.hackingarticles.in/finding-vulnerability-serverclient-using-nmap/)

