## 检查您的无线网络适配器是否支持监控模式和数据包注入

### 查看现有的适配器

`lsusb -vv`

### 将您的卡置于监控模式

`airmon-ng start wlan0`

`iwconfig`

### 测试您的卡进行数据包注入

`aireplay-ng --test wlan0mon`

### 通过攻击进行测试以确保一切正常 [^1]

`besside wlan0mon` [^2]

`besside -R 'target network' wlan0mon`



[原文](https://null-byte.wonderhowto.com/how-to/check-if-your-wireless-network-adapter-supports-monitor-mode-packet-injection-0191221/)

---

[^1]: 尝试使用 [Besside-ng](https://null-byte.wonderhowto.com/how-to/hack-wi-fi-automating-wi-fi-hacking-with-besside-ng-0176170/) 捕获WPA握手来实现上述两个步骤，Besside-ng 是一种用于WPA破解的通用且非常有用的工具，如果您的卡能够攻击，这也是一种很好的测试WPA网络方法
[^2]: 默认情况下，Besside-ng 会攻击范围内的所有内容，攻击非常嘈杂。Besside-ng旨在扫描连接设备的网络，然后通过注入解除认证数据包攻击连接，导致设备暂时断开连接。当它重新连接时，黑客可以使用设备交换的信息来尝试强制破解密码。