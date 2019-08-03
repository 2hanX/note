## Digital Forensics , Part 8 (More Windows Registry Forensics)

### Registry Keys

- `HKEY_CLASSES_ROOT`：用于打开文件的应用程序的配置信息
- `HKEY_CURRENT_USER`：当前登录用户的个人资料
- `HKEY_LOCAL_MACHINE`：配置信息，包括硬件和软件设置
- `HKEY_USERS`：包含所有已加载的用户配置文件
- `HKEY_CURRENT_CONFIG`：启动时系统的硬件配置文件

### The RecentDocs Key [^1]

- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`

### The TypedURLs Key [^2]

- `HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\TypedURLs`

### IP Addresses [^3]

- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`

### Start Up Locations in the Registry [^4]

- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`

### RunOnce Startup [^5]

- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce`

### Start Up Services [^6]

- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services`

### Start Legacy Applications [^7]

- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\WOW`

### Start When a Particular User Logs On [^8]

- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`

### USB Storage Devices [^9]

- `HK_Local_Machine\System\ControlSet00x\Enum\USBSTOR`

### Mounted Devices [^10]

- `HKEY_LOCAL_MACHINE\System\MountedDevices`



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-8-more-windows-registry-forensics-0162609/)

---

[^1]: 通过文件扩展名跟踪系统上最近使用或打开的文档
[^2]: 列出用户使用IE访问的最后一个URL。这可能会揭示在违规行为中使用的恶意恶意软件的来源，或者在民事或政策违规类型的调查中，可能会泄露用户正在寻找的内容
[^3]: 注册表还可以跟踪用户界面的IP地址。请注意，可能有许多接口，但注册表项可以跟踪每个接口的IP地址和相关信息
[^4]: 系统启动时设置的应用程序或服务。恶意软件通常设置为每次系统重新启动时自动启动以保持攻击者连接。这些信息可以在几十个位置的注册表中找到
[^5]:	如果黑客只是希望软件在启动时运行一次，则可以在此处设置子项
[^6]: 系统启动时启动的所有服务的位置。如果密钥设置为2，则服务自动启动; 如果设置为3，则必须手动启动服务; 如果密钥设置为4，则禁用该服务
[^7]: 运行旧版16位应用程序时，运行程序的位置
[^8]: 当特定用户登录时运行这些值
[^9]: 查看系统是否被安装了键盘记录器或查看攻击者用USB驱动器删除了哪些机密信息。我们可以找到插入和使用USB存储设备的证据
[^10]: 查看计算机上安装的每个设备