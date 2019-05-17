## Windows7 注册表

### 自定义光驱名

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\DriveIcons\H\DefaultLabel`

### 注册表由多个文件构成，称为Hive文件（Windows7 上称为注册区）

| 键名            | Hive文件保存路径                                             | Hive文件名    |
| :-------------- | ------------------------------------------------------------ | ------------- |
| `HKLM\SAM`      | `%systemroot%\system32\config`                               | SAM           |
| `HKLM\SECURITY` | `%systemroot%\system32\config`                               | SECURITY      |
| `HKLM\SOFTWARE` | `%systemroot%\system32\config`                               | SOFTWARE      |
| `HKLM\SYSTEM`   | `%systemroot%\system32\config`                               | SYSTEM        |
| `HKU\.DEFAULT`  | `%systemroot%\system32\config`                               | DEFAULT       |
| `HKU\S-15-18`   | `%USERPROFILE%`                                              | ntuser.dat    |
| `HKU\S-15-18_`  | `%userprofile%\local settings\classes application data\microsoft\windows` | userclass.dat |

### 设置屏保

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Screensavers`

1. Bubbles(气泡): 
   - MaterialGlass : DWORD : 0 : 气泡不透明 | 1 ： 气泡透
   - ShowBubbles : DWORD : 1 : 设置显示气泡时的背景，当前桌面上 | 0 : 清除画面内容，全黑画面显示气泡
   - ShowShadows : DWORD : 1 : 显示气泡的阴影 | 0 : 不显示阴影
2. Ribbons(彩带):
   - NumRibbons : DWORD : 设置彩带出现的彩带数量
   - Blur : DWORD : 设置彩带是否要淡出效果 ： 0 : 不淡出，最终屏幕变白
3. Mystify(变换线):
   - NumLines : DWORD : 设置变换线的数量

### BMP图片文件显示缩略图

`HKEY_CLASSES_ROOT\Paint.Picture\DefaultIcon `[^1]

### 创建快捷方式时不显示“快捷方式”文字

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer`

`link : REG_BINARY : 00 00 00 00`

### 删除快捷方式图标上的小箭头

`HKEY_CLASSES_ROOT\lnkfile `[^2]

### 在桌面上显示Windows版本

`HKEY_CURRENT_USER\Control Panel\Desktop `[^3]

### 开机时显示登录信息、管理员打造公告栏

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon`

- 显示的标题文字：`LegalNoticeCaption `：输入字符串
- 显示的内容：`LegalNoticeText `: 输入文字

### 系统时钟显示问候语

`HKEY_CURRENT_USER\Control Panel\International`

- 修改 `sLongDate `：`'今天是'yyyy'年'M'月'd'日! 每天好心情'`

### 隐藏桌面的“回收站”图标

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer`

- 新建项名： `HideDesktopIcons`
- 在其下新建项 ： `NewStartPanel`
- 新建DWORD值： 名称：`{645FF040-5081-101B-9F08-00AA002F954E}`
- 类型：`REG_DWORD`
- 数据：`1`（8进制）

### 修改系统的用户名、公司名

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion` [^4]

### 打开注册表时保持在根目录

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Applets\Regedit`

- 删除 `LastKey` 中的值，输入空字符串
- 修改权限：右键 `Regedit `子键，执行 “权限”命令，选择“Administrators”权限，勾选“拒绝”两个复选框

### 默认开启数字键（小键盘）

`HKEY_CURRENT_USER\Control Panel\Keyboard`

- 修改 `InitialKeyboardIndicators `的值 ：`2 `： 开启：`0 `： 关闭
- 修改权限

### 改变系统时钟的显示格式

`HKEY_CURRENT_USER\Control Panel\International`

- 修改 `s2359 `：下午 ：现在是下午
- 修改 `sTimeFormat `： H:mm:ss ：H时mm分ss秒

### 将搜狗输入法设为切换输入法的第一位

`HKEY_CURRENT_USER\Keyboard Layout\Preload` [^5]

### 添加启动程序到鼠标右键

`HKEY_CLASSES_ROOT\Directory\Background\shellex\ContextMenuHandlers`

### 关闭Aero Peek功能（鼠标移至右下角透明框几秒后显示桌面）的几秒延迟

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced` [^6]

### 编辑右键“新建”菜单中的文件类型（例如添加 html 文件格式）

`HKEY_CLASSES_ROOT\.htm`

- 添加项名：`ShellNew`
- 在其下添加名称：`NullFile`
- 类型：`REG_SZ`
- 值：空

### 从快捷菜单直接启动命令提示符

`HKEY_CLASSES_ROOT\Folder\shell`

- 新建项名：`Cmd`
- 修改默认名称数据为 “命令提示符”
- 新建项名：`command`
- 修改默认名称数据为 `cmd.exe /k pushd %L`

### 强迫应用程序关闭后，完整释放系统资源

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer`

- 新建 DWORD 值：名称： `AlwaysUnloadDLL`
- 类型：`REG_DWORD`
- 数据：`1`

### 强制将USB设为只读，数据带不走

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control` [^7]

- 新建项名：`StorageDevicePolicies`
- 新建DWORD 32位值
- 名称：`WriteProtect`
- 类型：`REG_DWORD`
- 数据：`1`

### 隐藏特定的磁盘驱动器（例如隐藏F盘）

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer`

- 新建键值：`NoDrives`：`REG_DWORD`: `32`

- 参考：

  | 要隐藏的磁盘代号 | 键值内容（10进制） |
  | ---------------- | ------------------ |
  | 不隐藏           | 0                  |
  | A                | 1                  |
  | B                | 2                  |
  | C                | 4                  |
  | D                | 8                  |
  | E                | 16                 |
  | F                | 32                 |
  | G                | 64                 |
  | 隐藏所有磁盘     | 67108863           |


### 强化BitLocker加密的安全性

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft`

- 添加项名：`FVE`

- 在其下添加键名：`EncryptionMethod` : REG_DWORD: `2`

  | 键值 | 内容                         |
  | ---- | ---------------------------- |
  | 1    | 128位加强型AES加密（默认值） |
  | 2    | 256位加强型AES加密           |
  | 3    | 128位AES加密                 |
  | 4    | 256位AES加密                 |

### 封锁Windows所有的快捷键功能

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer`

- 新建键值：`NoWinKeys`：REG_DWORD：`1`

  | 快捷键        | 功能         |
  | ------------- | ------------ |
  | `Win + space` | Aero Peek    |
  | `Win + P`     | 切换到投影仪 |
  | `Win + ↑`     | 窗口最大化   |
  | `Win + ←`     | 窗口靠左放大 |
  | `Win + +`     | 放大镜       |

### 删除“运行”命令的操作记录信息

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU`

- 删除 `RunMRU`

### 关闭默认打开的共享文件夹（远程访问这些文件夹，需要管理员权限） [^8]

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\LanmanServer\Parameters`

- 新建键值：`AutoShareServer `: REG_DWORD : `0`
- 新建键值：`AutoShareWKs `: REG_DWORD : `0`

### 自动清除打开过的文件记录

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer`

- 新建键值：`ClearRecentDocsOnExit `: REG_DWORD : `1`

### 彻底清除访问过的网页记录

`HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\TypedURLs`

- 删除表项

### 注册表使“显示隐藏的文件、文件夹和驱动器”失效

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced`

- 新建项：`Folder`
- 在其下新建项：`Hidden`
- 在其下新建项：`SHOWALL`
- 在其下添加键值：`CheckedValue `: REG_DWORD : `0`



---

[^1]: 将默认的数据改为` %1`。如果安装其他的图片处理软件，使得BMP图片文件不是与画图软件产生关联，那么更改 `Paint.Picture` 键的值就没有作用，所以必须找到该应用程序的对应键：`HKEY_CLASSES_ROOT\.bmp`，查看 `默认` 的数据是否是 `Paint.Picture`
[^2]: 删除 `IsShortcut : REG_SZ `
[^3]: 修改 `PaintDesktopVersion` 的值为 `1`
[^4]: 修改： `RegisteredOrganization`、`RegisteredOwner`
[^5]: 修改 `1 `：`00000804`
[^6]: 添加项名： `DesktopLivePreviewHoverTime `：`REG_DWORD `: `0`
[^7]: 重新启动后，如果要把数据写入U盘，系统会出现错误信息不让数据写入，同时除了U盘外，包括读卡器、MP3等通过USB传输的设备都不能写入
[^8]: `\\127.0.0.1\c$`  : 显示C盘下的文件内容