### 为操作做抓包标记（抓包技巧）

```commonlisp
ping <IP> -n 1 -l 1 #打标记；发送ICMP包长度为1个字节：a
操作步骤1
ping <IP> -n 1 -l 2 #打标记；发送ICMP包长度为2个字节：ab
操作步骤2
ping <IP> -n 1 -l 3 #打标记；发送ICMP包长度为3个字节：abc
操作步骤3
```

#### 显示顺序

在`wireshark→Preferences→Appearance→Columns`下添加列标记，`Fields`填`data.len`

### 调整日期格式[^1]

`View`→`Time Display Format`→`Date and Time of Day`

### 分析网络性能和连接问题

`Analyze`→`Expert Information`

### 衡量服务器性能

`Statistics`→`Service Response Time`

### 推测负载状况

`Statistics`→`TCP Stream Graph`→`Time-Sequence Graph (Stevens)`

`Statistics`→`Capture File Properties`→`Average bytes/s` …

### 搜索功能[^2]

`Packet bytes` + `Narrow(ASCII)` + `String `+`"payload"`

### MTU[^3]

`MTU` =` MSS（[1460] bytes）` + `TCP header（20 bytes）`+ `IP header（20 bytes）`

### 发送窗口和接收窗口大小

- 接收窗口大小
  1. 每个包的TCP层都含有 `window size value`（`Win=`）信息，表示告诉对方自己的接收窗口大小
  2. 一旦对方收到该包，就会把自己的发送窗口限制在该值之内
- 真正TCP接收窗口大小（==ACK==包中）
  - TCP标头为窗口大小分配的值是两个字节，意味着接收窗口的最高可能数值为==65535（2^16-1）==字节；如今此窗口大小不足以提供最佳流量；因此[RFC 1323](https://www.ietf.org/rfc/rfc1323.txt)中引入了TCP选项，使  **TCP接收窗口能够以指数方式增加，特定功能称为TCP窗口缩放**
  - `Calulated window size`= `{2^(shift count)|[Window size scaling factor]}*(Window size value)`
  - `Window Scale`：向对方声明一个`Shift count`
- 增大接收窗口
  - `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters`
  - `TcpWindowSize :DWORD:[0-0x3FFFC0000]`
  - Ref. [KB224829](https://support.microsoft.com/en-us/help/224829/description-of-windows-2000-and-windows-server-2003-tcp-features)
- 发送窗口大小
  - 难以确认，情况复杂多变

### 发送窗口和`MSS`的关系[^4]

### 超时重传

- 拥塞点 [^6]

- 拥塞窗口 [^5]

  - 慢启动、拥塞避免

- `RTO`[^7]

  - wireshark解包查看分析`RTO`
    - `Analyze `→`Expert information` →`Note（retransmission）`
  - 降低`RTO`（default：3000ms）
    - `netsh interface tcp show global`
    - `netsh interface tcp set global InitialRto=1000`
    - `netsh interface tcp set global MaxSynRetransmissions=3`


### 丢包触发快速重传[^8]

- 方案1：把3、4、5、6、7、8号等6个包都重传一遍；效率低；早期解决方案
- 方案2（`NewReno`）：接收方收到重传过来的2号包之后，回复一个Ack 3，发送方重传3号包；接收方收到重传的3号包之后，因丢包的窟窿已补满所以回复一个Ack 9，从此发送方就可以传新的包（9、10、11、……）；丢包量很大时将花费多个RTT（往返时间）来重传丢失的包
- 方案3（`SACK`）：接收方发送Ack 2号包的时，顺便把收到的包号告诉发送方；例如：`SACK=13635(SLE)-16515(SRE)`和`Ack=12195`两个条件综合起来，发送方就知道`13635～16515`
  已收到，而前面的`12195～13634`没收到



---

[^1]: 把 Wireshark 的抓包时间调成跟服务器一样的时间格式；此外，如果在其他时区的服务器上抓包，然后下载到自己的电脑上分析，最好把自己电脑的时区设成跟抓包的服务器一样
[^2]: 当遇到应用层错误时，使用 `Ctrl+F`搜索关键字；如怀疑包里含有“payload”词
[^3]: 大多数网络的MTU是1500字节，但有些网络启用巨帧（Jumbo Frame），能达到9000字节；TCP三次握手时，双方都会把自己的MSS（Maximum Segment Size）告诉对方；发包的大小是由MTU较小的一方决定
[^4]: 发送窗口决定一口气能发多少字节；`MSS`决定这些字节要分多少个包发完；例如：发送窗口为`16000`字节，如果 `MSS`=`1000`字节，就需要发送`16000/1000=16`个包
[^5]: 发送方的发送窗口受接收方的接收窗口和网络影响，而网络的影响方式复杂多变；避免网络拥塞的最佳策略就是保证发送方的发送窗口尽可能接近拥塞点
[^6]: 导致网络拥塞的数据量称为拥塞点
[^7]: 从发出原始包到重传该包的这段时间（发送方）
[^8]: 例：数据包2号和3号丢失，但1、4、5、6、7、8号都到达了接收方并触发Ack 2；发送方只能通过Ack 2知道2号包丢失，但并不知道还有哪些包丢失，在重传了2号包之后，接下来应该传哪一个数据包？