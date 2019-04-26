## 使用Syncthing在两台机器之间安全地同步文件 [^1]

### Install （Kali）

`apt-get update && apt-get install apt-transport-https -V` [^2]

`curl -s https://syncthing.net/release-key.txt | sudo apt-key add -` [^3]

`echo 'deb https://apt.syncthing.net/ syncthing stable' >> /etc/apt/sources.list` [^4]

```bash
# Kali的存储库提供旧版本的Syncthing。要确保在安装和更新Syncthing时始终使用syncthing.net软件包，请在/etc/apt/preferences.d/目录中创建以下引脚优先级。将整个下面的命令复制到终端并按Enter键
echo 'Package: *
Pin: origin apt.syncthing.net
Pin-Priority: 1001' > /etc/apt/preferences.d/syncthing
# 最后，更新APT并安装Syncthing
apt-get update && apt-get install syncthing
```

### Install （Windows）

`scoop install syncthing`

###  Start Syncthing

`systemctl --user start syncthing.service` [^5]

`ssh -L 9999:127.0.0.1:8384 -p 22 user@YOUR-VPS-IP-ADDRESS` [^6]

### Configure Syncthing  （Kali）

- 添加远程设备
- 粘贴设备ID
- “默认文件夹” 选项输入VPS IP地址时将端口**：22000** 附加到IP后面 - 这是默认的Syncthing侦听端口

`systemctl --user enable syncthing` [^7]

### Configure Syncthing  （Windows）

[window_config link](https://github.com/syncthing/docs/blob/master/users/autostart.rst#windows)

[原文](https://null-byte.wonderhowto.com/how-to/securely-sync-files-between-two-machines-using-syncthing-0185999/)

---

[^1]: [Syncthing](https://github.com/syncthing/syncthing)是一种跨平台，私有，轻量级文件同步（Dropbox）替代方案。使用Syncthing，您的所有数据都不会存储在计算机以外的任何其他位置。没有中央服务器可能受到损害（合法或非法）。您基本上是删除中间人（Dropbox）并直接在您的计算机之间同步敏感文件。Syncthing的用途不仅限于此方案。在本地Windows机器和MacBook之间，我们可以安全地同步浏览器书签，密码管理器文件，操作系统备份，媒体等等。
[^2]: 使用下面的 **apt-get** 命令确保安装 **apt-transport-https** 软件包。这将允许您从Syncthing开发人员安全地获取包和更新
[^3]: 导入Syncthing版本PGP密钥。这些用于安全地签署包，并帮助防止攻击者操纵Syncthing包
[^4]: 使用 **echo**命令将 [Syncthing存储库](https://apt.syncthing.net/) 添加到APT源
[^5]: 此命令不会产生任何输出。使用Web浏览器，导航到 **http://127.0.0.1:8384/** 以查看Kali中的Syncthing用户界面
[^6]: 从本地计算机打开新的浏览器选项卡并导航到**http://127.0.0.1:9999**。这实际上创建了一个安全隧道，允许远程用户访问在VPS环回（127.0.0.1）地址上运行的服务（Syncthing）。
[^7]: 使用**systemctl**命令在每次启动时启用Syncthing