## Digital Forensics , Part 14 (Live Memory Forensics)

### Why Capture Live Memory?

- 在某些情况下，取证调查员需要获取实时记忆的图像。因为RAM是易失性的，一旦系统关机，RAM中的任何信息都将丢失。此信息可能包括密码，正在运行的进程，打开的套接字，剪贴板内容等。在关闭系统或传输系统之前，必须捕获所有这些信息
- 此外，许多硬盘驱动器都使用 [TrueCrypt](http://truecrypt.sourceforge.net/) 等加密，密码驻留在RAM中。如果硬盘驱动器是加密的，那么捕获易失性数据就更为重要，因为没有它，取证调查员可能无法获得硬盘驱动器信息
- 有许多工具可以从内存中捕获数据，但是一家公司AccessData已经免费提供了多年的 [FTK Imager](http://accessdata.com/product-download)

### Using the FTK Imager to Capture Memory

- `File -> Capture Memory `

### Volatility Memory Analysis Tool [^1]

- Kali 下默认已安装 Volatility
- [下载 Volatility](http://www.volatilityfoundation.org/#!25/c1f29)

### Using Volatility for Analysis

`kali > cd /usr/share/volatility`

`kali > python vol.py -h`

`kali > python vol.py imageinfo -f /location of your imagefile`

`kali > python vol.py imageinfo -f /root/Desktop/memdump.mem`

### Using the Profile

`kali > python vol.py --profile Win7SP1x64 hiveinfo -f /locationof your image/` [^2]



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-14-live-memory-forensics-0168337/)

---

[^1]: 分析内存捕获与硬盘驱动器分析略有不同。内存分析的一个优点是能够在系统捕获时实际重新创建嫌疑人正在做的事情。
[^2]: 列出包括SAM的注册表配置单元，使用 **hiveinfo** 插件。Volatility能够列出所有的配置单元，包括它们在RAM中的虚拟和物理位置