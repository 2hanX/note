## 使用Nmap生成扫描报告

`nmap 192.168.1.113 -oN /root/Desktop/nmap`

`nmap -p80 192.168.1.113 -oX  ~/Desktop/nmap.xml` [^1]

`xsltproc nmap.xml -o nmap.html`

`nmap 192.168.1.113 -oS /root/Desktop/nmap`

`nmap 192.168.1.113 -oG /root/Desktop/nmap` [^2]

`nmap 192.168.1.113 -oA /root/Desktop/nmap` [^3]

---

[^1]: XML代表可扩展标记语言是Nmap支持的通常已知的树状结构文件格式。将扫描结果保存为XML格式的文件; 添加选项-oX <filename>
[^2]: 包含grepable格式以帮助用户从日志中提取信息而无需编写解析器，因为此格式旨在使用标准UNIX工具进行读取/解析。要将扫描结果保存为grepable格式的文件，请添加-oG <filename>选项
[^3]: Nmap支持别名选项-oA <basename>，它将扫描结果保存为所有可用格式 - 普通格式，XML格式和grepable格式。将使用扩展名.nmap，.xml和.gnmap生成不同的文件