## Digital Forensics , Part 15 (Parsing Out Key Info from Memory)

### Getting the List of Processes

`kali > python vol.py --profile Win7SP1x64 pslist -f /root/Desktop/memdump.mem`

`kali > python vol.py -h`

### Getting the Running DLLs

`kali > python vol.py --profile Win7SP1x64 dlllist -f /root/Desktop/memdump.mem`

### Getting the Contents of the System's Clipboard

`kali > python vol.py --profile Win7SP1x64 clipboard -f /root/Desktop/memdump.mem` [^1]

### Getting a Timeline of Events

`kali > python vol.py --profile Win7SP1x64 timeliner -f /root/Desktop/memdump.mem`

### Looking for Malware in the Memory

`kali > python vol.py --profile Win7SP1x64 malfind -f /root/Desktop/memdump.mem`



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-15-parsing-out-key-info-from-memory-0169435/)

---

[^1]: 解析出的这些信息都是十六进制的，必须翻译成ASCII