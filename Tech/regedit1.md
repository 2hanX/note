## Windows7 注册表(1)

### 清除曾经搜索过的关键字

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWhellQuery`

- 删除该项

### 取消系统/文件夹下的右击弹出快捷菜单功能

`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer`

- 新建键值： `NoTrayContextMenu`： REG_DWORD：`1`
- 新建键值：`NoViewContextMenu`：REG_DWORD：`1`

### 禁止打开“系统属性”对话框

`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer`

- 新建键值： `NoProperties_MyComputer`： REG_DWORD：`1`

### U盘/移动硬盘的安插禁用

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\USBSTOR`

- 修改键值：`Start` : REG_DWORD：`3`或者`4`

### 关闭注册表编辑器，避免被修改

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Polices\System`

- 新建键值：`DisableRegistry` : REG_DWORD : `1`

### 解除“注册表编辑器”的禁令

1.  借助第三方软件恢复：**Tweak Manager**、**Ultimate Windows Tweaker**、**X-Setup Pro** 等
2. 使用 Administrator账号登录系统，利用注册表编辑器的加载Hive控制文件功能，删除原有用户注册表中的 `DisableRegistryTools `键值

### 关闭“程序和功能”功能，避免重要程序被删除

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Polices`

- 新建项：`Programs`
- 在其下新建键值：`NoProgramsAndFeatures`：REG_DWORD：`1`

### 打开上帝模式

1. 在桌面新建文件夹
2. 重命名文件夹为 `GodMode.{ED7BA470-8E54-465E-825C-99712043E01C} `[^1]

---

## 组策略编辑器（gpedit.msc)

### 避免用户随意清除浏览记录

- 切换到 `用户配置\管理模板\Windows组件\Internet Explorer\删除浏览的历史记录`
- 定位到“设置”，选择 “阻止删除xxx”，即可启动

### 使用 [Active Registry Monitor](https://www.devicelock.com/arm/) 创建注册表的记录点

- 记录软件安装前后的注册表内容
- 对比记录点，查看软件卸载残留的键值

### 使用 Registry Life 最佳化注册表设置

- 清除无效的注册表内容
- 优化注册表内容



---

[^1]: 虽然名字响亮，但实际上是控制面板的高级版，将原先被归类在一起的功能设置全部分为各个图标