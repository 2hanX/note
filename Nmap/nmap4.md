## 了解Nmap Ping扫描指南（防火墙旁路）

`nmap -sn 192.168.1.105 --disable-arp-ping`

`nmap -sP 192.168.1.105 --disable-arp-ping`

### **阻止Ping扫描扫描**

```bash
sudo iptables -I INPUT -p ICMP -j DROP
sudo iptables -I INPUT -p tcp --tcp-flags ALL ACK --dport 80 -j DROP
sudo iptables -I INPUT -p tcp --tcp-flags ALL SYN --dport 443 -j DROP
# 在IPTABLES中放入一些防火墙规则来丢弃ICMP数据包，端口443上的TCP SYN数据包和端口80上的TCP ACK，这将阻止Ping扫描扫描
```

### **使用TCP SYN Ping绕过Ping扫描过滤器**

`nmap -sP -PS 192.168.1.104 --disable-arp-ping` [^1]

### **阻止TCP SYN Ping扫描**

```bash
sudo iptables -I INPUT -p tcp --tcp-flags ALL SYN -j DROP
# 会阻止NMAP TCP SYN Ping探测，以便它无法识别活动主机的状态
```

### **使用TCP ACK Ping绕过TCP SYN Ping**

`nmap -sP -PA 192.168.1.104 --disable-arp-ping` [^2]

`nmap -sP -PA443 192.168.1.104 --disable-arp-ping`

### **阻止TCP ACK Ping扫描**

```bash
sudo iptables -I INPUT -p tcp --tcp-flags ALL ACK -j DROP
```

### **使用ICMP Echo绕过TCP ACK Ping**

`nmap -sP -PE 192.168.1.104 --disable-arp-ping` [^3]

### **阻止ICMP Echo Ping扫描**

```bash
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```

### **使用ICMP Timestamp Ping绕过ICMP Echo Ping**

`nmap -sP -PP 192.168.1.104 --disable-arp-ping` [^4]

### **阻止ICMP Ping扫描**

```bash
sudo iptables -I INPUT -p ICMP -j DROP
```

### **使用UDP Ping绕过ICMP Ping扫描**

`nmap -sP -PU 192.168.1.104 --disable-arp-ping`

### **阻止UDP和Ping扫描**

```bash
sudo iptables -I INPUT -p ICMP -j DROP
sudo iptables -I INPUT -p tcp --tcp-flags ALL ACK --dport 80 -j DROP
sudo iptables -I INPUT -p tcp --tcp-flags ALL SYN --dport 443 -j DROP
sudo iptables -I INPUT -p UDP -j DROP
```

### **使用协议扫描绕过UDP和Ping扫描**

`nmap -sP -PO 192.168.1.104 --disable-arp-ping` [^5]

### **阻止IP协议Ping扫描**

```bash
sudo iptables -I INPUT -p ICMP -j DROP
sudo iptables -I INPUT -p tcp --tcp-flags ALL ACK --dport 80 -j DROP
sudo iptables -I INPUT -p tcp --tcp-flags ALL SYN --dport 443 -j DROP
sudo iptables -I INPUT -p UDP -j DROP
sudo iptables -I INPUT -p IP -j DROP
```

### **使用No Ping Scan绕过IP协议Ping**

`nmap -sP -PN 192.168.1.104 --disable-arp-ping` [^6]

---

[^1]: 使用 TCP 扫描和 TCP SYN 数据包来绕过防火墙规则，`-PS`默认在端口80上发送TCP SYN包，可以指定端口来改变默认操作，比如`-PS443`
[^2]: 使用TCP ACK数据包进行ping扫描，`-PA`默认在端口80上发送TCP ACK数据包，可以通过指定端口来更改它，如`-PA443`
[^3]: 使用ping扫描和ICMP数据包，`-PE`发送ICMP 回送请求包：回显请求[ICMP类型8]、回显应答[ICMP类型0]
[^4]: 使用ping扫描和ICMP数据包，`-PP`发送ICMP 时间戳请求包：时间戳请求包[ICMP类型13]、时间戳应答包[ICMP类型14]
[^5]: 使用Ping扫描，我们可以在ICMP，TCP和UDP被阻止时识别实时主机，`-PO`属性在其IP报头中发送具有特定协议号的IP数据包，如果没有准确的协议，则默认为ICMP（协议1），IGMP（协议2）和IP-in-IP发送多个IP数据包（协议4）
[^6]: 当上面所有的Ping扫描都无法识别 Host 的状态是 up 还是 down 时，最后一个最好的选项 No Ping，使用`-PN / -P0 / -Pn`