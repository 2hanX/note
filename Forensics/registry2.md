### Forensics Evidence about System Access through User Activities [^1]

#### MRU[^2]

`reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU /s`

`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePidlMRU`[^5]

`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`[^4]

`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU`[^6]

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management`[^7]

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`[^8]

#### UserAssist[^3]

`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count`

| GUID                                      | 作用                                         |
| ----------------------------------------- | -------------------------------------------- |
| `{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA} ` | 已访问的应用程序，文件，链接和其他对象的列表 |
| `{F4E57C4B-2036-45F0-A9AB-443BCFE33D9F}`  | 列出用于启动程序的快捷方式链接               |

#### Tools

- [UserAssistView Tools](https://www.nirsoft.net/utils/userassist_view.html)
- [DCode](https://www.digital-detective.net/dcode/)

#### Disable log

- `NoEncrypt:DWORLD:1`
- `NoLog:DWORD:1`
- 清除：控制面板→文件资源管理器选项→常规→清除文件资源管理器历史记录

### Forensic Keys Related to Firewall and Security Center[^9]

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Security Center`

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy`[^10]

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`下反病毒软件服务

| Start（Value） | Description  |
| -------------- | ------------ |
| 2              | 随系统自启动 |
| 3              | 手动         |
| 4              | 禁用         |

#### Ref.

- [registry analysis](https://www.andreafortuna.org/2017/10/18/windows-registry-in-forensic-analysis/)



---

[^1]: 大部分用户活动记录在 ==ntuser.dat==
[^2]: Most Currently Used (MRU) ：储存用户输入的命令等
[^3]: 有关用户活动的重要信息；**ROT 13**（向前偏移13个位置）加密方式
[^4]: 最近从**Windows资源管理器**打开的本地或网络文件，并且仅存储二进制形式的文件名
[^5]: 最近通过Windows资源管理器对话框（**打开/保存**对话框）打开或保存的文件的列表；例如最近打开的文件（例如.txt，.pdf，htm，.jpg）或从Web浏览器中保存的文件
[^6]: 最近使用的程序可执行文件名，以及用于打开或保存程序的文件的文件夹路径
[^7]: **ClearPagefileAtShutdown**：该值指定Windows是否应在计算机关闭时清除分页文件（*默认情况下，Windows不会清除分页文件*）；值为1时比较可疑
[^8]: 已安装的程序；**DisplayName**  - 程序名称；**UninstallString**  - 应用程序卸载组件的文件路径，间接引用应用程序安装路径
[^9]: 入侵者入侵后最先步骤是修改文件时间戳、禁用系统所有通知、关闭防火墙和反病毒软件
[^10]: StandardProfile；DomainProfile