## 5 ways to Banner Grabbing [^1]

`nmap -sV --script=banner 192.168.1.106`

`nmap -Pn -p 80 -sV --script=banner 192.168.1.106`

`curl -s -I 192.168.1.106 | grep -e "Server: "` [^2]

`telnet 192.168.1.106 22` [^3]

`nc -v 192.168.1.106 22` [^4]

`dmitry -b 192.168.1.106` [^5]

[原文](https://www.hackingarticles.in/5-ways-banner-grabbing/)

---

[^1]: **banner**是指从主机接收的文本消息。通常包含有关服务的信息，例如版本号。
[^2]: HTTP Banner
[^3]: SSH Banner
[^4]: SSH Banner
[^5]: Dmitry（Deepmagic信息收集工具）是用C编码的UNIX /（GNU）Linux命令行应用程序.Dmitry能够收集尽可能多的关于主机的信息。基本功能可以收集可能的子域，电子邮件地址，正常运行时间信息，tcp端口扫描，whois查找等。