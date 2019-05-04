## Use Photon Scanner to Scrape Web OSINT Data [^1]

### Knowing What to Search For 

- 可以寻找漏洞，快速解析其中的内容，并以易于理解的方式呈现给专业人员
- 能够自动识别和提取某些类型的数据，例如页面脚本，电子邮件地址以及可能意外暴露的重要密码或API密钥
- 可以回顾过去。使用 [Wayback Machine](https://archive.org/web/)上记录的保留以前的网页状态作为搜索的“种子”，从现已解散的网站中删除所有网址作为进一步抓取的来源
- 生成连接到域的所有内容的可视DNS映射

### What You'll Need

` apt-install python3`

### Download & Install Photon

`pip install tld requests`

```bash
git clone https://github.com/s0md3v/Photon.git
cd Photon
```

### View Photo's Options

`python3 photon.py -h`

### Map DNS Information

`python3 photon.py -u https://www.priceline.com/ --dns`

### Extract Secret Keys & Intel

`python3 photon.py -u https://www.pbs.org/ --keys -t 10 -l 3`

### Make Requests with a Third Party Using Ninja Mode [^2]

`python3 photon.py -u https://www.pbs.com/ --keys -t 10 -l 1 --ninja`

### Photon Makes Scanning Through URLs Lightning-Fast [^3]



[原文](https://null-byte.wonderhowto.com/how-to/use-photon-scanner-scrape-web-osint-data-0194420/)

---

[^1]: 专为OSINT设计的Web爬虫（Photon）来收集在线目标上的信息
[^2]: 假设我们从敏感的IP地址开始工作，例如警察局，政府办公室，甚至只是你的家，你不希望目标知道正在访问他们的网站。您可以使用 **--ninja** 标记在自己和目标之间设置距离，该标志会将您的请求发送到第三方网站，为您提出请求，并传递响应。v1.3.2 版本使用了代理模块，去掉了 Ninja 模式
[^3]: 当涉及到爬行数百个URL以获取信息时，Photon可以轻松抓取大量子域或多个目标，从而可以在重新调整阶段扩展您的研究。借助内置的智能选项，可以解析和搜索各种数据，如电子邮件地址和重要的API密钥，Photon可以捕获目标的小错误，从而揭示大量有价值的信息