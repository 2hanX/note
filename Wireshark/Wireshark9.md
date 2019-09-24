## Wireshark 命令行工具

为了方便地使用Wireshark附带的命令行工具，在此之前建议将本地Wireshark安装目录的路径添加到系统环境变量中，接下来让我们看看Wireshark附带的以下更有用的命令行实用程序：

- `tshark`
- `capinfos`
- `mergecap`
- `editcap`

> 每个命令行程序都可加 `-h` 参数查看详细操作手册

### tshark

`tshark`用于捕获数据包，并且也可以在我们没有使用交互式用户界面的权限时或者担心数据丢包的情况下使用。因为在网络上充斥大量数据包的情况下，Wireshark的捕获引擎可能无法通过接口进行大量快速捕获数据包，并且随时可能会崩溃或无响应。因此使用`tshark`捕获此类流量是一个明智的选择

#### 选择网卡

`tshark -D`[^1]

#### 开始抓取数据包

`tshark –i 2` [^2]

#### 停止抓取数据包

- 人工停止：`Ctrl + C`
- 自动停止：`tshark –i 2 –a duration:10` [^3]

#### 保存数据包

`tshark –i 2 –w Capture.pcap`

#### 过滤

- 抓取时过滤：`tshark –i 2 –f "dhcp" –w DHCP.pcap`
- 本地读文件时过滤：`tshark -2 –R "http" –r Traffic.pcap –w HTTP.pcap`

> 常用过滤命令：
>
> - `ftp.request.command == "PASS"`
> - `ftp.request.command == "USER"`
> - `http.request.method == "GET"`

#### 协议分层统计

`tshark –r Traffic.pcap –qz io,phs`[^4]

### capinfos

`capinfos`用于读取一个或多个捕获文件并返回一些或全部可用统计信息

`capinfos Traffic.pcap`

### mergecap

`mergecap`主要用于将多个捕获文件组合到单个输出文件中

`mergecap capture1.pcap capture2.pcap -w traffic.pcap`

### editcap

`editcap` 主要用于修改捕获文件，例如将大文件拆分为多个文件；从文件中删除重复数据包；将捕获文件从一种格式转换为另一种格式

`editcap –d Duplicates.pcap NoDuplicates.pcap` [^5]

`editcap -c 10 ftp.pcap ftp1.pcap`[^6]

`editcap -F`[^7]

`editcap -F pcap example.erf transter.pcap`

### Ref.

- [wireshark docs](https://www.wireshark.org/docs/man-pages/)



---

[^1]: 列出本地网络接口
[^2]: 选择想要使用并开始截获流量的接口（这里以太网卡序号是2）
[^3]: 开始抓取数据包`10s`后自动停止
[^4]: 显示 `Protocol Hierarchy Statistics`
[^5]: 尝试删除重复数据包；将当前分组的长度和MD5散列与先前的四个分组进行比较，如果找到匹配项，则跳过当前数据包；此选项相当于使用选项`-D 5`
[^6]: 分割`ftp.pcap`文件，保存为`ftp1_xxx.pcap`文件集，每个文件有10个数据包
[^7]: `-F` 命令可以列出所有支持的格式