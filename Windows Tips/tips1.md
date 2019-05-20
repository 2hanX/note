### 使用windows 命令行创建一个空的文本文件

1. 使用 `fsutil `创建一个文件大小为3000比特的文本

   `fsutil file createNew C:\Desktop > 2.txt 3000`

2. 使用 `echo `创建一个文本

   `echo > demo.txt`

3. 使用 `type` 创建一个文本

   `type nul > 4.txt`

### 使用windows 命令行查看文件内容

1. `type demo.txt`
2. `type demo.txt | findstr hello`

