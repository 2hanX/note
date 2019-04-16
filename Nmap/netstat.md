## netstat 命令

`netstat -t` [^1]

`netstat -a` [^2]

`netstat -u`

`netstat -n`

`netstat -l` [^3]

`netstat -at`

`netstat -r`[^4]

`netstat -e` [^5]

`netstat -p` [^6]

`netstat -s` [^7]

`netstat -c` [^8]

`netstat -lx` [^9]

`netstat -i` [^10]

`netstat -anp | grep port/name/pid`

`lsof -i:port`

`lsof -i | grep pid`



---

输出结果：

从整体上看，netstat的输出结果可以分为两个部分：

一个是Active Internet connections，称为有源TCP连接，其中"Recv-Q"和"Send-Q"指%0A的是接收队列和发送队列。这些数字一般都应该是0。如果不是则表示软件包正在队列中堆积。这种情况只能在非常少的情况见到。

另一个是Active UNIX domain sockets，称为有源Unix域套接口(和网络套接字一样，但是==只能用于本机通信==，性能可以提高一倍)。
Proto显示连接使用的协议,RefCnt表示连接到本套接口上的进程号,Types显示套接口的类型,State显示套接口当前的状态,Path表示连接到套接口的其它进程使用的路径名。

---

[^1]: 仅显示tcp相关选项
[^2]: 显示所有选项，默认不显示LISTEN相关
[^3]: 仅列出有在 Listen (监听) 的服務状态
[^4]: 显示路由信息，路由表
[^5]: 显示扩展信息，例如uid等
[^6]: 显示建立相关链接的程序名
[^7]: 按各个协议进行统计
[^8]: 每隔一个固定时间，执行该netstat命令
[^9]: 只列出所有监听 UNIX 端口
[^10]: 显示网络接口列表