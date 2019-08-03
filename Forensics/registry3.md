###  Keys with Forensic value

| Key                                                          | Description                                     |
| ------------------------------------------------------------ | ----------------------------------------------- |
| ==Software==                                                 |                                                 |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\` | Install Apps                                    |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall` | <span style="color:red">Uninstall Apps</span>   |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\ProfileList` | <span style="color:red">SID Profiles</span>     |
| `HKLM\SOFTWARE\MICROSOFT\WindowsNT\CurrentVersion\SystemRestore` | Restore Points                                  |
| `HKLM\SOFTWARE\Classes`                                      | Class Registration and Files  Association       |
| `HKCU\Software\Classes`                                      | Per-user settings                               |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Comdlg32` | Most Currently Used Files                       |
| `KHCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU` | MRU order                                       |
| `HKCU\SOFTWARE\Microsoft\Search Assistant\ACMru`             | Recently Search                                 |
| `HKLM\Software\Microsoft\Command Processor`                  | <span style="color:red">AutoRun</span>          |
| `KHCU\Software\Microsoft\Protected Storage System Provider`  | Windows Protected Storage                       |
| `HKLM\System\CurrentControlSet\Services\Tcpiip\Parameters\interfaces` | IP                                              |
| [`HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon`](title:"DefaultUserName；DefaultDomainName") | <span style="color:red">Last Logon Users</span> |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32` | Last Visited MRU                                |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSaveMRU` | Open and Save of Recent Files                   |
| `HKCU\Software\Microsoft\Windows\SearchAssistant\ACMru`      | Files and words searched                        |
| ==Hardware==                                                 |                                                 |
| `HKLM\SYSTEM\CurrentControlSet\HardwareProfile\XXXX`         | Current Hardware  Settings                      |
| `HKLM\SYSTEM\MountedDevices`                                 | Mounted devices                                 |
| `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR`                 | USB Devices                                     |
| ==Network==                                                  |                                                 |
| `HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\GUID` | IP Address and Gateway                          |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\MapNetworkDriveMRU` | Mapped Network Devices                          |

### Forensic Evidence from Security Identifiers （SID）[^1]

[S](title="表示以下字符串是SID")-[1](title="修订号")-[5](title="授权级别，范围从0到5")-[21](title="本地或域标识符")-[602162358-839522115-1957994488](title="本地计算机标识符")-[1004](title="相对标识符，也是本地计算机或域内的唯一编号")



---

[^1]: SID 与用户名的关系映射存储在SAM中；也可手动查看：`ProfileImagePath`

