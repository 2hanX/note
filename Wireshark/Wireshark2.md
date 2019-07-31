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

`View→Time Display Format→Date and Time of Day`



---

[^1]: 把 Wireshark 的抓包时间调成跟服务器一样的时间格式；此外，如果在其他时区的服务器上抓包，然后下载到自己的电脑上分析，最好把自己电脑的时区设成跟抓包的服务器一样