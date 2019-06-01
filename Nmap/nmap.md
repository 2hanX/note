## 带定时参数的Nmap扫描

`nmap -T4 –d -p21-25 192.168.1.139`

`nmap -p21-25 192.168.1.139 --max-retries 0 ` [^1]

```bash
# 在目标计算机上应用一个小型防火墙规则，以便在数据包速度更快时阻止数据包
sudo iptables -I INPUT -p tcp -m state --state NEW -m recent --set
sudo iptables -I INPUT -p tcp -m state --state NEW -m recent --update --seconds 1 --hitcount 1 -j DROP 
```

`nmap -p21-25 192.168.1.139 --max-retries 5`

`nmap -p21-25 192.168.1.139 --host-timeout 10ms`

`nmap -sP 192.168.1.139 --host-timeout 100ms`

`nmap -sP 192.168.1.1/24 --min-hostgroup 3 --max-hostgroup 3` [^3]

`nmap -p21-25 192.168.1.139 --scan-delay 11s` [^4]

`nmap -p21-25 192.168.1.139 --max-rate 2` [^5]

`nmap -p21-25 192.168.1.139 --min-rtt-timeout 5ms` [^6]

`nmap -p21-25 192.168.1.139 --initial-rtt-timeout 50ms` [^7]

---

[^1]: `--max-retries`：指定在端口上重新发送数据包以检查其是打开还是关闭的次数。如果`--max-retries`设置为0，则数据包将仅在端口上发送一次，并且不会重试
[^3]: `hostgroup`属性指明一次扫描网络中指定数量的主机，需要设定要扫描的最小主机数或最大主机数或两者
[^4]: `--scan-delay`：用于延迟在指定时间发送的数据包，它在避开基于时间的防火墙方面非常有用
[^5]: `--max-rate`：指定要发送数据包的速率，也就是一次发送的最大数据包数
[^6]: `--min-rtt-timeout`：指定数据包返回应答时使用的最小时间值
[^7]: `--initial-rtt-timeout`：指定数据包返回应答所用的初始值，返回时间可以大于或小于`initial-rtt-timeout`，因为`max-rtt-timeout`和`min-rtt-timeout`指定数据包返回应答的时间范围，但数据包尝试在`initial-rtt-timeout`中指定的时间内返回应答