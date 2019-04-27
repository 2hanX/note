###### 处理信号

- 中断进程 [^1]
- 暂停进程 [^2]

###### 捕获信号 [^3]

```bash
#!/bin/bash
trap "echo 'Sorry! I have trapped Ctrl-C'" SIGINT
echo This is a test script
count=1
while [ $count -le 10 ];do
	echo "Loop #$count"
	sleep 1
	count=$[ $count+1 ]
done
echo "This is the end of the test script"
```

###### 脚本退出时捕获

```bash
#!/bin/bash
trap "echo Goodbye..." EXIT
count=1
while [[ $count -le 5 ]];do
	echo "Loop #$count"
	sleep 1
	count=$[ $count+1 ]
done

# 因为SIGINT信号并没有出现在trap命令的捕获列表中，当按下`Ctrl+C`组合键发送SIGINT信号时，脚本就退出了。但在脚本退出前捕获到了EXIT，于是shell执行了trap命令
```

###### 修改或移除捕获

```bash
#!/bin/bash
trap "echo 'Sorry... Ctrl-C is trapped.'"	SIGINT
count=1
while [[ $count -le 5 ]]; do
	echo "Loop #$count"
	sleep 1
	count=$[ $count+1 ]
done

trap "echo 'I modified the trap! '"	SIGINT
count=1
while [[ $count -le 5 ]]; do
	echo "Second loop #$count"
	sleep 1
	count=$[ $count+1 ]
done
```

```bash
#!/bin/bash
# 删除已设置好的捕获。只需要在trap命令与希望恢复默认行为的信号列表之间加上两个破折号就行了

trap "echo 'Sorry... Ctrl-C is trapped.'"	SIGINT
count=1
while [[ $count -le 5 ]]; do
	echo "Loop #$count"
	sleep 1
	count=$[ $count+1 ]
done

# trap "echo 'I modified the trap! '"	SIGINT
trap -- SIGINT
echo "I just removed the trap"
count=1
while [[ $count -le 5 ]]; do
	echo "Second loop #$count"
	sleep 1
	count=$[ $count+1 ]
done

# 也可以在trap命令后使用单破折号来恢复信号的默认行为。单破折号和双破折号都可以正常发挥作用。
```

###### 后台运行脚本

` ./test4.sh &` [^4]

###### 在非控制台下运行脚本 [^5]

`nohup ./test1.sh &`

###### 查看作业

```bash
#!/bin/bash
echo "Script Process ID: $$"
count=1
while [[ $count -le 10 ]]; do
	echo "Loop #$count"
	sleep 8
	count=$[ $count + 1 ]
done
echo "End of Script..."

#`$$`变量来显示Linux系统分配给该脚本的PID
```

###### 重启停止的作业 [^6]

- 后台模式重启作业
  - `bg [作业号]`
- 前台模式重启作业
  - `fg [作业号]`

###### 调整谦让度 [^7]

`nice -n 10 ./test.sh > test.out &`

`nice -n -10 ./test.sh > test.out &`

`nice -10 ./test.sh &`

`ps -p 4493 -o pid,ppid,ni,cmd`

`renice -n 10 -p 5033` [^8]

用**`at`**命令来计划执行作业 [^9]

`at -f ./test.sh now`

`at -m -f ./test.sh now`

###### 列出等待的作业

`atq`

###### 删除作业

`atrm [作业号]`

###### 安排需要定期执行的脚本

###### `cron`时间表

`min hour dayofmonth month dayofweek command` [^10]

`15 10 * * * command` [^11]

`15 16 * * 1 command` [^12]

`00 12 1 * * command` [^13]

`    15 10 * * * /home/rich/test4.sh > test4out` [^14]

`00 12 * * * if [‘date +%d -d tomorrow‘ = 01 ] ; then ; command` [^15]

###### 构建`cron`时间表

`crontab -l`

`ls /etc/cron*ly `

######  `anacron` 程序

`sudo cat /var/spool/anacron/cron.monthly`

`sudo cat /etc/anacrontab`

`period delay identifier command`

###### 使用新 `shell` 启动脚本

P367

[下一篇](shell6.md)

---

[^1]:  ==Ctrl+C== 组合键会生成`SIGINT (2 终止进程)`信号，并将其发送给当前在shell中运行的所有进程
[^2]: ==Ctrl+Z==  组合键会生成一个`SIGTSTP (18 停止或暂停进程，但不终止进程)`信号，停止shell中运行的任何进程
[^3]: 也可以不忽略信号，==在信号出现时捕获它们并执行其他命令==。`trap`命令允许你来指定shell 脚本要监看并从shell中拦截的Linux信号。如果脚本收到了`trap`命令中列出的信号，该信号不再 由shell处理，而是交由本地处理;`trap`命令的格式:  `trap commands signals`
[^4]: 当后台进程运行时，它仍然会使用==终端显示器==来显示`STDOUT`和`STDERR`消息
[^5]: 使用`nohup`命令来实现在终端会话中启动shell脚本，然后让脚本一直以后台模式运行到结束，即使你退出了终端会话。如果使用`nohup`运行了另一个命令，该命令的输出会被追加到已有的`nohup.out`文件中。当运行位于同一个目录中的多个命令时一定要当心，因为所有的输出都会被发送到同一个` nohup.out`文件中，结果会让人摸不清头脑
[^6]: 在bash作业控制中，可以将已停止的作业作为==后台进程或前台进程重启==。前台进程会接管你当前工作的终端，所以在使用该功能时要小心了
[^7]: 在多任务操作系统中(Linux就是)，内核负责将CPU时间分配给系统上运行的每个进程。调 度优先级(scheduling priority)是内核分配给进程的CPU时间(相对于其他进程)。在Linux系统 中，由shell启动的所有进程的调度优先级默认都是相同的。调度优先级是个整数值，从==-20(最高优先级==)到==+19(最低优先级)==。默认情况下，bash shell 以==优先级0==来启动所有进程。
[^8]: 改变系统上==已运行==命令的优先级。只能对属于你的进程执行`renice`; 只能通过`renice`降低进程的优先级;  `root`用户可以通过`renice`来任意调整进程的优先级。如果想完全控制运行进程，必须以`root`账户身份登录或使用`sudo`命令。
[^9]: `at [-f filename] time` P362; 使用`e-mail`作为`at`命令的输出极其不便。`at`命令利用`sendmail`应用程序来发送邮件。如 果你的系统中没有安装`sendmail`，那就无法获得任何输出!因此在使用`at`命令时，最好在脚本中对`STDOUT`和`STDERR`进行重定向
[^10]: Linux系统使用`cron`程序来安排要定期执行的作业。`cron`程序会在后台运行并检查一个特殊的表(被称作==cron时间表==)
[^11]: 在每天的10:15运行一个命令
[^12]: 在每个月每周一4:15 PM运行一个命令
[^13]: 每个月的第一天中午12点执行命令
[^14]: 命令列表必须指定要运行的命令或脚本的全路径名。
[^15]: 设置一个在每个月的最后一天执行的命令;每天中午12点来检查是不是当月的最后一天，如果是，`cron`将会运行该命令