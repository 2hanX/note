### 延迟确认与Nagle算法[^1]

### 临界窗口（拥塞控制）

- Westwood

- Vegas算法[^2]

- Compound算法[^3]

  - Win7：

    ```shell
    netsh interface tcp set global congestionprovider=ctcp #开启
    netsh interface tcp set global congestionprovider=none #关闭
    ```

  - Win10：

    ```powershell
    Get-NetTCPSetting #检查当前的拥塞提供程序
    Set-NetTCPSetting -SettingName InternetCustom -CongestionProvider CTCP
    ```

- Ref. [TCP/IP tweaks](https://www.speedguide.net/articles/windows-8-10-2012-server-tcpip-tweaks-5077)

### DNS

- 递归查询：`nslookup www.example.com`
- 迭代查询[^4]：`dig www.example.com + trace`

### 在Wireshark中解密SSL / TLS流量

- 使用**SSLKEYLOGFILE**只能解密客户端发往服务器的SSL流量
- 解密服务器端发往客户端的SSL流量需要服务器端的秘钥
- 解密后的协议为HTTP2

Ref. 

- [decrypt_ssl](https://www.joji.me/zh-cn/blog/walkthrough-decrypt-ssl-tls-traffic-https-and-http2-in-wireshark/)
- [wiki_wireshark](https://wiki.wireshark.org/TLS?action=show&redirect=SSL)

### 只抓包头

`Capture`→`Options`→`Snaplen（200 bytes）`

`dumpcap -s 200 -c 10`

`capinfos dump.pcapng`



---

[^1]: 一些交互式应用会传递大量的小包，这些小包的负载可能只有几bytes，如果大量传递这种小包，会严重降低网络利用率，还可能造成网络拥塞；==Nagle算法原理==：在发出去的数据还没有被确认之前，假如又有小数据生成，那就把小数据先收集起来，凑满一个MSS或者等收到确认后再发送；延迟确认和Nagle算法并没有直接提高性能，只是减少部分确认包，减轻网络负担
[^2]: 之前的算法都是在发生丢包后才调节拥塞窗口，而Vegas却通过监控网络状态来调整发包速度，从而实现真正的“拥塞避免”；==理论依据==：当网络状况良好时，数据包的RTT（往返时间）比较稳定，这时候就可以增大拥塞窗口；当网络开始繁忙时，数据包开始排队，RTT就会变大，这时候就需要减小拥塞窗口；==最大优势==在于在拥塞真正发生之前，发送方已经能通过RTT预测到，并且通过减缓发送速度来避免丢包的发生；==弊端==在于当环境中存在Vegas和其他算法时，使用Vegas的发送方可能是性能最差的，因为它最早探测到网络繁忙，而主动降低自己的传输速度
[^3]: 优势的结合
[^4]: 客户端先查到根服务器的地址，再从根服务器查到权威服务器，然后从权威服务器查，直到返回想要的结果