了解Nmap数据包跟踪

`nmap -sn 192.168.1.103 --packet-trace` [^1]

`nmap -sn 192.168.1.103 --reason` [^2]

`nmap -PS -p22 192.168.1.103 --packet-trace` [^3]

`nmap -PS -p22 192.168.1.103 --packet-trace --disable-arp-ping`

`nmap -sP -PE 192.168.1.103 --packet-trace --disable-arp-ping` [^4]

`nmap -p22 192.168.1.103` [^5]

`nmap -sT -p22 192.168.1.103 --packet-trace`

---

[^1]: 选项`-sn / -sP`用于扫描ping，它们尝试识别网络中的实时主机。选项`-packet-trace`观察网络数据包
[^2]: 选项 `-reason `：枚举来自主机网络的响应
[^3]: 选项 `-PS`：默认在端口80上发送TCP-SYN包，后面跟`-P22`则表示在端口22上发送TCP-SYN包
[^4]: 选项`-PE`：发送ICMP回应请求包
[^5]: Nmap隐身扫描分析：捕获默认nmap扫描的网络数据包，也称为秘密扫描，它遵循TCP半通信