## Use the Buscador OSINT VM for Conducting Online Investigations [^1]

### OSINT Investigations

- OSINT 研究工具允许我们访问社会中产生的大量数据
- 汇集他们自己和其他调查人员使用的最有效的 OSINT 工具和定制脚本，该虚拟机具有的另一个重要特点是安全性，隐秘性以及能够轻松保存调查期间发现的数字取证证据的能力

### A VM for Hackers, Researchers & Investigators

- Buscador OSINT VM 基于 Ubuntu 而不是 Debian
- Buscador 不包括 Kali 所拥有的强大的网络武器，而是将一组有用的 OSINT  隐私和捕获工具手工挑选到一个隐秘的包中。因为避免检测是调查人员和黑客分享的目标，Buscador 预装了 [Tor](https://null-byte.wonderhowto.com/how-to/tor/) 和其他有用的隐私工具
- Buscador VM 还可以从任何可用计算机上的USB驱动器启动，也可以加载到硬盘上并直接启动。无论您是否随身携带个人设备，都可以灵活地在任何可访问计算机的地方使用它
- Buscador 鼓励良好的研究习惯，并使研究人员能够在调查中找到更多线索。并且预装了一些熟悉的工具，比如 [Maltego](https://null-byte.wonderhowto.com/collection/maltego/)，[Recon-ng](https://null-byte.wonderhowto.com/how-to/hack-like-pro-reconnaissance-with-recon-ng-part-1-getting-started-0169854/)，Creepy，[Spiderfoot](https://null-byte.wonderhowto.com/how-to/use-spiderfoot-for-osint-gathering-0180063/)，[TheHarvester](https://null-byte.wonderhowto.com/how-to/scrape-target-email-addresses-with-theharvester-0176307/)，[Sublist3r](https://null-byte.wonderhowto.com/how-to/quickly-look-up-valid-subdomains-for-any-website-0184426/)

### What You'll Need

- [官方网站](https://inteltechniques.com/buscador/)
- Mike Bazzell 的书 "[开源智能技术](https://www.amazon.com.au/Open-Source-Intelligence-Techniques-Information/dp/1984201573/?tag=wnbau-22)"

### Import & Configure the Virtual Appliance

- 在 VirtualBox 虚拟机里导入下载的 Buscador.ova 文件
- 在 "高级"下，将"共享剪贴板"更改为"双向"
- 单击 "系统"选项卡，在"主板"下，将大约一半的系统 RAM 添加到虚拟机。然后，单击"显示"选项卡，然后单击"屏幕"，将"视频内存"增加到至少128 MB，以便正确显示视频和其他数字证据
- 单击"存储"选项卡，然后单击左下角的加号图标，选择"添加光盘驱动器"，然后选择"保留空"选项
- 单击"共享文件夹"选项卡，然后选择右侧的加号图标并创建或选择要用于将Buscador 中的证据保存到计算机上的文件夹。选择此选项后，请确保将文件夹设置为"自动安装”

### Run Buscador for the First Time

- default: `osint : osint`
- 登录并启动桌面后，单击 VirtualBox 菜单顶部的"设备"选项卡，然后选择 "Insert Guest Additions CD Image" 以在 Buscador 中显示 CD。如果它不自动运行，请选择桌面上的 CD，然后单击"运行软件"以自动运行 Guest Additions 安装程序。安装完成后，重新启动虚拟机
- `sudo adduser osint vboxsf` [^2]

### Take Advantage of Browser Extensions

- #### Firefox Browser Add-Ons

- #### Chrome Browser Extensions



[原文](https://null-byte.wonderhowto.com/how-to/use-buscador-osint-vm-for-conducting-online-investigations-0186611/)

---

[^1]: Buscador 是一个虚拟机，其中包含大量有用的 OSINT 工具，并简化了在线研究
[^2]: 将 `osint `用户添加到 `vboxsf `用户组