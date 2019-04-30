## Digital Forensics  , Part 7 (Windows Sysinternals)

### [Install Sysinternals ](https://technet.microsoft.com/en-us/sysinternals/bb842062.aspx) [^1]

- **AccessChk** - 允许您查看用户和组对文件，目录，注册表项等的访问类型。
- **AccessEnum** - 文件系统和注册表安全设置的完整视图。
- **AdExplorer** - Active Directory查看器和编辑器。
- **AdInsight** - 用于对Active Directory应用程序进行故障排除的LDAP实时监控工具。
- **AdRestore** - 能够还原已删除的Active Directory对象。
- **Autologon** - 轻松配置自动登录机制。
- **Autoruns** -配置为在启动时运行的程序显示。
- **BgInfo** - 显示有关台式计算机的相关信息，例如计算机名称，IP地址等。
- **CacheSet** - 用于操作系统文件缓存的工作集参数的applet。
- **ClockRes** - 显示系统时钟的分辨率。
- **Contig** - 对指定文件进行碎片整理。
- **Coreinfo** - 显示逻辑处理器和物理处理器之间的映射。
- **Ctrl2Cap** - 用于过滤系统键盘类驱动程序的内核模式设备驱动程序。
- **DebugView** - 监视本地系统上的调试输出。
- **Desktops** - 允许您组织最多四个虚拟桌面。
- **Disk2vhd** - 创建物理磁盘的VHD（虚拟硬盘）版本。
- **DiskExt** - 返回有关卷分区所在磁盘的信息。
- **DiskMon** - 记录并显示所有硬盘活动。
- **DiskView** - 硬盘的图形化地图。
- **DiskUsage（DU）** - 报告指定目录的磁盘空间使用情况。
- **EFSDump** - 允许您查看谁有权访问加密文件。
- **FindLinks** - 报告指定文件的文件索引和硬链接。
- **Handle** - 显示有关任何进程的打开句柄的信息。
- **Hex2dec** - 将十六进制转换为十进制，反之亦然。
- **Junction** - 创建联结（组合来自多个位置的目录的符号链接）。
- **LDMDump** - 让我们仔细检查存储在系统的磁盘副本中的内容。
- **ListDLLs** - 报告加载到进程中的DLL。
- **LiveKd** - 允许您运行Kd和Windbg内核调试程序。
- **LoadOrder** - 显示系统加载设备驱动程序的顺序。
- **LogonSessions** - 列出当前活动的登录会话。
- **MoveFile** - 转储挂起的重命名/删除值的内容。
- **NTFSInfo** - 显示有关NTFS卷的信息。
- **PageDefrag** - 显示您的分页文件和注册表配置单元已碎片化。
- **PendMoves** - 转储挂起的重命名/删除值的内容。
- **PipeList** - 列出管道。
- **PortMon** - 监视并显示所有串行和并行端口活动。
- **ProcDump** - 监控CPU峰值。
- **Process Explorer** - 显示有关加载哪些句柄和DLL进程的信息。
- **Process Monitor** - 显示实时文件系统，注册表和进程/线程活动。
- **PsExec** - 允许您在远程系统上执行进程。
- **PsGetSid** - 允许您将SID转换为其显示名称，反之亦然。
- **PsInfo** - 收集有关本地或远程系统的关键信息，包括内核版本和内存量。
- **PsPing** - 实现ping功能。
- **PsKill** - 可以杀死本地和远程系统上的进程。
- **PsList** - 显示有关进程，内存和线程的信息。
- **PsLoggedOn** - 显示谁在本地或远程计算机上使用什么资源。
- **PsLogList** - 允许您在安全凭证不允许的情况下登录远程系统。
- **PsPasswd** - 允许您更改本地或远程系统上的帐户密码。
- **PsService** - Windows的服务查看器和控制器。
- **PsShutdown** - 允许您注销控制台用户或锁定控制台等。
- **PsSuspend** - 允许您暂停本地或远程系统上的进程。
- **RAMMap** - 一种物理内存使用情况分析工具，用于查看Windows如何分配物理内存。
- **RegDelNull** - 允许您搜索和删除注册表项。
- **Registry Usage (RU)** - 报告注册表空间使用情况。
- **RegJump** - 直接打开[Regedit](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-5-windows-registry-forensics-0160561/)到指定的注册表路径。
- **RootkitRevealer** - 检测rootkit。
- **SDelete** - 允许您删除一个或多个文件/目录或清除驱动器上的可用空间。
- **ShareEnum** - 允许您锁定文件共享。
- **ShellRunas** - 允许您在不同帐户下启动程序。
- **SigCheck** - 显示文件版本号，时间戳和数字签名详细信息。
- **Streams** - 允许您查看哪些NTFS文件具有与之关联的备用流。
- **Strings** - 搜索指定字符串的文件。
- **Sync** - 允许您将所有文件系统数据刷新到磁盘。
- **TCPView** - 显示系统上所有TCP和UDP端点的详细列表。
- **VMMap** - 流程虚拟和物理内存分析工具。
- **VolumeID** - 允许您更改FAT和NTFS磁盘的ID。
- **WhoIs** - 为指定的域名或IP地址执行注册记录。
- **WinObj** - 显示NT对象管理器的名称空间的信息。
- **ZoomIt** - 用于技术演示的屏幕缩放和注释工具。

### Open Process Explorer  [^2]

### Use Process Monitor to Examine a Process

- 双击它并打开它的属性
- 单击“权限”以查看谁拥有此进程的权限

### Check Strings [^3]



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-7-windows-sysinternals-0162080/)

---

[^1]: 安装后，您可以查看 SysinternalsSuite 文件夹并查看可用的众多工具。以下是工具列表（按字母顺序排列）及其功能
[^2]: Process Explorer （procexp）列出了每个进程及其子进程，CPU使用情况，专用字节，工作集，PID，描述和公司。如果我们怀疑有恶意软件感染，我们通常可以在Process Explorer中找到它的证据
[^3]: 在我们从进程中收集到的众多信息中，我们可以提取嵌入在此进程中的任何ASCII字符串。通常，我们可以找到有关进程的关键信息，包括开发人员留下的任何注释。