## Digital Forensics , Part 12 (Windows Prefetch Files)

### Prefetch System [^1]

- 微软为了提高Windows的性能，预读系统按照其名称预取系统预期用户将需要的文件，并将其加载到内存中，从而使文件的“获取”更快，更高效
- Windows中预取系统的优点在于它可以显示有关用户正在做的大量信息，即使他们足够智能以试图掩盖他们的踪迹。自Windows XP和2003（那些是相同的版本）以来，Prefetch一直是Windows操作系统的一部分，默认情况下它通过 Windows 10 启用。在服务器端，Windows Server 2003,2008和2012，应用程序预取很多都在注册表中启用
-  这些预读文件包含有关所用文件的元数据。此元数据包括诸如应用程序的最后使用日期，应用程序文件的存储位置，应用程序使用次数以及取证调查员的其他一些有用信息等。这可能是试图证明嫌疑人实际使用了涉及犯罪的应用程序的关键信息，例如浏览器或Word等文档创建软件，即使它已从系统中删除

### Location of PreFetch Files [^2]

`C:\windows\prefetch`

### Analyzing Prefetch

- 查看文件基本信息，可以看到它们被修改的最后日期，这通常意味着上次使用这些应用程序数据的时间
- 分析软件：[WinPrefetchView](http://www.nirsoft.net/utils/win_prefetch_view.html)，EnCase，FTK和Oxygen
- *exe*的路径、运行的次数、上次运行的次数、程序使用的每个文件的路径

### Disabling Prefetch & Superfetch

- cmd: `compmgmt.msc`
- service : `Superfetch`
- startup type: `Disabled`
- click : `Stop`



---

### Refer 

- [是否可以删除Prefetch文件夹](https://answers.microsoft.com/en-us/windows/forum/windows_vista/can-i-delete-the-data-in-the-prefetch-folder/c54a8e56-c8cf-451d-a88f-07f06a2f2d54)



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-12-windows-prefetch-files-0167643/)

---

[^1]: 预读系统
[^2]: 目录下可以看到所有的预读文件，这些是以*.pf* 结尾的文件和数据库文件（*.DB*）