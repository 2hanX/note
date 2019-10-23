### 前言

首次遇到 USB 流量分析的题，并且是 16 年的 IceCTF的题 - Intercepted Conversations Pt.1

### 0x0  协议统计

`tshark -r intercept1.pcapng -qz io,phs`

根据结果我们知道该流量包是捕获的 USB HID流量，在此之前我们需要简单了解下 USB协议

### 0x1 USB协议

详细分析USB通信协议不在文章的范围内，这里选择比较重要的地方做简单介绍

- 键盘流量

  - 数据包的数据长度字节数：8（击键信息集中在第3个字节）

    ```
    BYTE1 --
           |--bit0:   Left Control是否按下，按下为1 
           |--bit1:   Left Shift  是否按下，按下为1 
           |--bit2:   Left Alt    是否按下，按下为1 
           |--bit3:   Left GUI    是否按下，按下为1 
           |--bit4:   Right Control是否按下，按下为1  
           |--bit5:   Right Shift 是否按下，按下为1 
           |--bit6:   Right Alt   是否按下，按下为1 
           |--bit7:   Right GUI   是否按下，按下为1 
    BYTE2 -- 暂不清楚，有的地方说是保留位
    BYTE3--BYTE8 -- 这六个为普通按键
    ```

    

- 鼠标流量

  - 数据包的数据长度字节数：4

    ```
    BYTE1 -- 代表按键
           |--bit7:   1/0   表示 Y 坐标的变化量超出 -256 ~ 255 的范围,0表示没有溢出  
           |--bit6:   1/0   表示 X 坐标的变化量超出 -256 ~ 255的范围,0表示没有溢出  
           |--bit5:   Y   坐标变化的符号位，1表示负数，即鼠标向下移动  
           |--bit4:   X   坐标变化的符号位，1表示负数，即鼠标向左移动  
           |--bit3:     1
           |--bit2:     1表示中键按下  
           |--bit1:     1表示右键按下  
           |--bit0:     1表示左键按下  
    BYTE2 -- X坐标变化量，与byte的bit4组成9位符号数,负数表示向左移，正数表右移。用补码表示变化量  
    BYTE3 -- Y坐标变化量，与byte的bit5组成9位符号数，负数表示向下移，正数表上移。用补码表示变化量 
    BYTE4 -- 滚轮变化
    ```

### 0x2 提取键盘数据

`tshark -r intercept1.pcapng -T fields -e usb.capdata -Y 'usb.capdata && usb.transfer_type == 0x01 && frame.len == 72' > usbdata`

根据键盘协议我们知道如果捕获的字符以 `02 `或 `20 `开头，则表示捕获的密钥以大写字母表示，比如：

```
20000a0000000000 : G
00000c0000000000 : i
0000070000000000 : d
02000c0000000000 : I
...
```

### 0x3 编写脚本

```python
mappings = { 0x04:"A",  0x05:"B",  0x06:"C", 0x07:"D", 0x08:"E", 0x09:"F", 0x0A:"G",  0x0B:"H", 0x0C:"I",  0x0D:"J", 0x0E:"K", 0x0F:"L", 0x10:"M", 0x11:"N",0x12:"O",  0x13:"P", 0x14:"Q", 0x15:"R", 0x16:"S", 0x17:"T", 0x18:"U",0x19:"V", 0x1A:"W", 0x1B:"X", 0x1C:"Y", 0x1D:"Z", 0x1E:"1", 0x1F:"2", 0x20:"3", 0x21:"4", 0x22:"5",  0x23:"6", 0x24:"7", 0x25:"8", 0x26:"9", 0x27:"0", 0x28:"\n", 0x2a:"[DEL]",  0X2B:"    ", 0x2C:" ",  0x2D:"-", 0x2E:"=", 0x2F:"[",  0x30:"]",  0x31:"\\", 0x32:"~", 0x33:";",  0x34:"'", 0x36:",",  0x37:"." }
nums = []
for line in open('.\\usbdata','r'):
    nums.append(int(line[4:6],16))
output = ""
for n in nums:
    if n == 0 :
        continue
    if n in mappings:
        output += mappings[n]
    else:
        output += '[none]'
print('output :\n',output)
```

### 0x4 分析结果

输出字符串是：` GIDIKY[,J0-P1V3;-X,3O7T-4LT,4T5]`，不过没有进行大小写转换，此外查表可以知道`0x2D`不仅代表`-`也代表下划线`_`，所以我们进行手工更改：`GidIKY{,j0_p1V3:_x,3O7T_4LT,4t5}`

看到这样一个字符串，还以为又是什么加密后的字符串，查阅互联网才知道这是 Dvorak键盘布局的键盘按键，转成Qwerty布局的键盘就得到 flag： `IceCTF{wh0_l1K3S_qw3R7Y_4NYw4y5}`

### Ref. 

- [HID Usage Tables 1.12](https://www.usb.org/document-library/hid-usage-tables-112)
- [Wireshark_USB](https://wiki.wireshark.org/USB)
- [USB协议分析](https://www.cnblogs.com/newjiang/p/9511331.html)
- [USB鼠标流量包取证工具](https://github.com/WangYihang/UsbMiceDataHacker)
- [USB键盘流量包取证工具](https://github.com/WangYihang/UsbKeyboardDataHacker)
- [深入理解USB流量数据包的抓取与分析](https://www.cnblogs.com/ECJTUACM-873284962/p/9473808.html)