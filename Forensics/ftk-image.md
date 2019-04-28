## Digital Forensics , Part 3 (Recovering Deleted Files)

### Windows File System's  [^1]

### NTFS [^2]

### FAT Filesystem [^3]

### DEMO

- Create a File

- Delete the File [^4]

### Create an Image [^5]

- [AccessData FTK Imager](https://accessdata.com/product-download/ftk-imager-version-4.2.1)

### Recover Deleted Files

- [RecoverMyFiles](http://www.recovermyfiles.com/data-recovery-software-download.php)



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-3-recovering-deleted-files-0149868/)

---

[^1]: 几乎所有现代Windows系统都使用NTFS文件系统，但较旧的系统仍可能使用FAT文件系统。实际上，如果您使用的是闪存驱动器，它可能使用较旧的FAT文件系统进行格式化，因为这样可以在包括Linux和Mac OS X在内的所有操作系统上使用
[^2]: NTFS文件系统被开发用于80年代末和90年代早期开发的“新”一代Windows，即Windows NT（NT文件系统）。微软转向与以前在数字设备公司（现在是惠普的一部分）合作开发Windows NT的操作系统开发人员合作来开发新的强大，可靠和安全的文件系统。随着Windows 2000的到来，这个文件系统最终取代了Windows系列中的其他文件系统，现在你只能在Windows系统上找到NTFS。NTSF有一个主文件表（MFT），用于跟踪硬盘上每个文件的位置。删除文件时，MFT只是将硬盘驱动器上的区域标记为可覆盖。在该区域实际被覆盖之前，该文件在硬盘驱动器上保持完好并且很容易恢复。即使它被覆盖，如果新文件不像以前的文件那么长，也可能仍然存在“slack sapce”。因此，如果您删除了一个4096kb的文件并用3000kb的文件覆盖它，那么slack space将超过1000kb，并且仍然可以被法医调查员恢复
[^3]: FAT是一个更老，更简单的文件系统。这是IBM于1981年使用微软DOS开发的第一台PC的文件系统。它有多种格式，包括FAT8，FAT12，FAT16和FAT 32.它是一个简单的文件系统，没有NTFS和Linux的ext2和ext3的所有安全性和其他功能。在本教程中，我们将介绍从FAT格式的闪存驱动器中恢复已删除的文件，但完全相同的工具和过程将适用于从NTFS，ext2，ext3，HFS，HFS +等恢复文件
[^4]: 右键单击恶意文件，然后选择删除。如果您将文件放在回收站中，您可以让法医调查员更容易恢复。回收站实际上只是一个文件夹，文件被移动，直到您清空回收站。在清空回收站之前，不会删除任何内容
[^5]: 法医调查员在检查您的计算机时将要做的第一步是逐位复制您的硬盘驱动器。有许多工具可以做到这一点，在Linux中，我们有dd命令，可以很好地制作逐位副本。文件备份和副本在法律上没有用，因为它们不会复制已删除的文件和文件夹，并且在许多情况下实际上会更改数据。大多数法医调查员使用商业工具。两种最受欢迎的是由Guidance Software提供的Encase和由Access Data提供的 Forensic Tool Kit（FTK）。