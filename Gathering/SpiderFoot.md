## Use SpiderFoot for OSINT Gathering [^1]

### Install SpiderFoot on Kali Linux

- [SpiderFoot官网](https://www.spiderfoot.net/download/)
- [项目地址](https://github.com/smicallef/spiderfoot)

### Launch SpiderFoot

- `nohup ./sf.py &`
- web: `127.0.0.1:5001`

### Add API Keys

- [官方文档](http://www.spiderfoot.net/documentation/)

- #### Honeypot Checker:

  1. Go to [http://www.projecthoneypot.org](http://www.projecthoneypot.org/).
  2. Sign up (free) and log in.
  3. Click on "Services," then "HTTP Blacklist."
  4. An API key should be listed.
  5. Copy and paste that key into the "Settings" -> "Honeypot Checker" section in SpiderFoot.

- #### SHODAN:

  1. Go to [http://www.shodanhq.com](http://www.shodanhq.com/).
  2. Sign up (free) and log in.
  3. Click on "Developer Center."
  4. On the far right, your API key should appear in a box.
  5. Copy and paste that key into the "Settings" -> "SHODAN" section in SpiderFoot.

- #### VirusTotal:

  1. Go to [http://www.virustotal.com](http://www.virustotal.com/).
  2. Sign up (free) and log in.
  3. Click on your username in the far right and select "My API Key."
  4. Copy and paste the key in the grey box into the "Settings" -> "VirusTotal" section in SpiderFoot

- #### IBM X-Force Exchange:

  1. Go to <https://exchange.xforce.ibmcloud.com/new>.
  2. Create an IBM ID (free) and log in.
  3. Go to your account settings.
  4. Click on "API Access."
  5. Generate the API key and password (you need both).
  6. Copy and paste the key and password into the "Settings" -> "X-Force" section in SpiderFoot.

- #### MalwarePatrol:

  1. Go to [http://www.malwarepatrol.net](http://www.malwarepatrol.net/).
  2. Create an account (free) and log in.
  3. Click on "Open Source" and scroll down to the bottom.
  4. Click on the "Free" link in the subscription pricing table.
  5. Click the free block lists link.
  6. You will receive a receipt ID.
  7. Copy and paste the receipt ID into the "Settings" -> "MalwarePatrol" section in SpiderFoot.

- #### BotScout:

  1. Go to [http://www.botscout.com](http://www.botscout.com/).
  2. Create an account (free) and log in.
  3. Under "Account Info," your API key will be there.
  4. Copy and paste the API key into the "Settings" -> "BotScout" section in SpiderFoot.

- #### Cymon.io:

  1. Go to [http://www.cymon.io](http://www.cymon.io/).
  2. Create an account (free) and log in.
  3. Under "My API Dashboard," your API key will be there.
  4. Copy and paste the API key into the "Settings" -> "Cymon" section in SpiderFoot.

- #### ...

### Launch a New Scan

- `NewScan -> Run Scan`

### Parsing the Information

- `Scans -> Graph`
- `...`

### SpiderFoot Is a Valuable OSINT Tool [^2]



[原文](https://null-byte.wonderhowto.com/how-to/use-spiderfoot-for-osint-gathering-0180063/)

---

[^1]: 模块化的跨平台OSINT（开源智能）收集工具。OSINT 是可以从公共来源收集的数据，它不仅限于互联网，还可以通过印刷媒体，政府记录，学术出版物等收集。根据您的目标，从互联网收集公开数据可能足以分析目标，这一切都取决于您的目标留下的网页足迹
[^2]: 可以认为SpiderFoot是第一阶段的信息收集工具。使用SpiderFoot发现的信息旨在成为深入洞察的起点。启用API密钥后，扫描程序将更详细地概述您要定位的系统，这种大规模的OSINT方法手动执行可能非常耗时，但是SpiderFoot让它变得快速