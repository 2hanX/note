### wireshark抓不到帧尾的FCS 4字节和以太网V2的前序8字节

- 在物理层上网卡要先去掉前导同步码和帧开始定界符，然后对帧进行CRC检验，如果帧校验和错，就丢弃此帧。如果校验和正确，就判断帧的目的硬件地址是否符合自己的接收条件（目的地址是自己的物理硬件地址、广播地址、可接收的多播硬件地址等），如果符合，就将帧交“设备驱动程序”做进一步处理。这时我们的抓包软件才能抓到数据。因此，抓包软件抓到的是去掉前导同步码、帧开始分界符、FCS之外的数据

  
    
