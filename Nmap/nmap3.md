## 了解Nmap数据包跟踪

`nmap -sn 192.168.1.103 --packet-trace` [^1]

`nmap -sn 192.168.1.103 --reason` [^2]

`nmap -PS -p22 192.168.1.103 --packet-trace` [^3]

`nmap -PS -p22 192.168.1.103 --packet-trace --disable-arp-ping`

`nmap -sP -PE 192.168.1.103 --packet-trace --disable-arp-ping` [^4]

`nmap -p22 192.168.1.103` [^5]

`nmap -sT -p22 192.168.1.103 --packet-trace`

---

[^1]: 属性**-sn / -sP**用于扫描ping，它们尝试识别网络中的实时主机。沿着nmap扫描使用**-packet-trace**我们可以观察网络数据包。
[^2]: 类似地，您也可以选择**-reason**选项和nmap命令来枚举来自主机网络的响应。
[^3]: **属性-PS默认在端口80上发送TCP SYN包; 我们可以通过用它指定端口来改变它，比如-P22。**
[^4]: 属性-PE发送ICMP回应请求包[ICMP类型8]并接收ICMP回送应答包
[^5]: **Nmap隐身扫描分析**让我们捕获默认nmap扫描的网络数据包，也称为秘密扫描，它遵循TCP半通信