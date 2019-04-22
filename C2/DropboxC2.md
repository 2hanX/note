## DropboxC2 （DBC2）

- Introduction [^1]
- Installation
  - `git clone //github.com/Arno0x/DBC2`
  - `cd DBC2/`
  - `pip install -r requirements.txt`
- Getting Dropbox API [^2]
- Exploiting Target
  - `python dropboxC2.py`
  - `publishStage dbc2_agent.exe`
- Sniffing Clipboard
- Capturing Screenshot
- Command Execution
- File Download



[原文](https://www.hackingarticles.in/command-and-control-with-dropboxc2/)

---

[^1]: DBC2是主要用于后渗透的工具



[^2]: 此工具使用Dropbox Server作为在目标计算机上运行代理的媒介。为此，此工具需要Dropbox API。首先在Dropbox上创建一个帐户。然后在创建帐户后，[转到此处](https://www.dropbox.com/developers/apps/create)的开发工具。选择“Dropbox API”，选择“App文件夹”，选择命名应用程序。然后单击“创建应用程序按钮”继续。这将跳转到另一个网页，在这里，转到O Auth 2部分，然后生成访问令牌。这将为此特定实用提供所需的Dropbox API。复制生成的访问令牌，然后转到我们之前克隆的目录下名为config.py的文件，打开它并将令牌粘贴为“defaultAccessToken”的值。