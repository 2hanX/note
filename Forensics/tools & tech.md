## Digital Forensics , Part 1 (Tools & Techniques)

### What Is Digital Forensics? [^1]

### The Digital Forensic Tools [^2]

- Guidance Software's EnCase Forensic
- Access Data's Forensic Tool Kit (FTK)
- Prodiscover
- The Sleuthkit Kit (TSK) [^3]
- Helix
- Knoppix
- ...

### The Forensic Tools Available in BackTrack

- sleuthkit
- truecrypt
- hexedit
- autopsy
- iphoneanalyzer
- rifiuti2
- ptk
- ...

### What Can Digital Forensics Do?

- 恢复已删除的文件，包括电子邮件
- 确定哪些计算机，设备和/或软件创建了恶意文件，软件和/或攻击
- 跟踪攻击的源IP和/或MAC地址
- 通过其签名和组件跟踪恶意软件的来源
- 确定拍照的时间，地点和设备
- 跟踪启用手机的设备的位置（启用或不启用GPS）
- 确定文件被修改，访问或创建的时间（MAC）
- 破解加密硬盘驱动器，文件或通信上的密码
- 确定犯罪者访问过的网站以及他下载的文件
- 确定嫌疑人使用的命令和软件
- 从易失性存储器中提取关键信息
- 确定攻击无线网络的人员以及未授权用户是谁
- ...

### What Is Anti-Forensics? [^4]

- **Hiding Data:** 隐藏数据包括加密和隐写术等内容
- **Artifact wiping:** 每次攻击都会留下签名或神器。有时尝试从受害者机器上擦除这些文物是明智的，这样就不会给调查员留下任何痕迹
- **Trail Obfuscation:** 一个合格的取证调查员可以追踪几乎任何远程攻击的IP地址或MAC地址。路径混淆是一种技术，可以将它们引导到另一个攻击源，而不是实际的攻击
- **Change the timestamp:** 更改文件时间戳（修改，访问和更改）来逃避取证工具的检测

### 

[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-1-tools-techniques-0149732/)

---

[^1]: 数字取证是确定谁应对数字入侵或其他计算机犯罪负责的领域。它使用广泛的技术来获取犯罪者的信息。它依赖于这样一个基本概念：无论发生数字入侵或犯罪，犯罪者都会无意中留下一些自己让调查人员可以追查到的作案痕迹。这些“bits”可能是日志文件中的条目，注册表的更改，黑客软件，恶意软件，已删除文件的残余等等。所有这些都可以提供线索和证据来确定其身份并导致捕获和逮捕黑客
[^2]: 大多数数字取证调查员依赖三个主要的商业数字取证套件。这三个套件由多个工具和报告功能组成，可能相当昂贵。虽然这些套件被执法部门广泛使用，但它们使用与免费开源套件相同或类似的技术，而没有花哨的界面。通过使用开源和免费套件，我们可以了解EnCase等工具如何在没有费用的情况下工作。EnCase是执法部门使用最广泛的工具，但不一定是最有效和最复杂的工具。这些工具旨在实现用户友好性，效率，认证，良好培训和报告
[^3]: 许多免费的开源取证套件，包括以下三个
[^4]: 反取证是一种技术，可用于混淆信息并躲避法医调查员的工具和技术