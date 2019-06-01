## 使用Nmap生成扫描报告

`nmap 192.168.1.113 -oN /root/Desktop/nmap`

`nmap -p80 192.168.1.113 -oX  ~/Desktop/nmap.xml` [^1]

`xsltproc nmap.xml -o nmap.html`

`nmap 192.168.1.113 -oS /root/Desktop/nmap`

`nmap 192.168.1.113 -oG /root/Desktop/nmap` [^2]

`nmap 192.168.1.113 -oA /root/Desktop/nmap` [^3]

---

[^1]: 将扫描结果保存为XML格式的文件
[^2]: 将扫描结果保存为 grepable 格式的文件，因为此格式旨在使用标准UNIX工具进行读取/解析
[^3]: 将扫描结果保存为所有可用格式 - 普通格式、XML格式和 grepable 格式