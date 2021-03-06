### 核心存储（Core Dump）[^1]

### 会产生核心转储的信号

Linux 中信号是一种异步事件处理的机制，每种信号对应有其默认的操作，你可以在[这里](http://man7.org/linux/man-pages/man7/signal.7.html)查看 Linux 系统提供的信号以及默认处理。默认操作主要包括：`忽略该信号（Ingore）`、`暂停进程（Stop）`、`终止进程（Terminate）`、`终止并发生core dump（core）`等。如果我们信号均是采用默认操作，那么以下列出几种信号，它们在发生时会产生 `core dump`:

| Signal              | Action | Comment                  |
| ------------------- | ------ | ------------------------ |
| `SIGQUIT`(`Ctrl+\`) | Core   | Quit from keyboard       |
| `SIGILL`            | Core   | Illegal Instruction      |
| `SIGABRT`           | Core   | Abort signal from abort  |
| `SIGSEGV`           | Core   | Invalid memory reference |
| `SIGTRAP`           | Core   | Trace/breakpoint trap    |

### 开启核心转储

- 输入命令 `ulimit -c` ，输出结果为` 0 `，说明默认是关闭的
- 输入命令 `ulimit -c unlimited` 即可在当前终端开启核心转储功能

### 永久开启核心转储功能

修改文件 `/etc/security/limits.conf` ，增加一行：

```shell
#<domain> 	<type> 	<item> 	<value>
*		    soft 	core 	unlimited
```

### 修改转储文件保存路径

- 修改生成的核心转储文件名存储模式为`core.[pid]`

  - 修改`/proc/sys/kernel/core_uses_pid`[^2]

  > `# echo 1 > /proc/sys/kernel/core_uses_pid`

- 控制生成核心转储文件的
  保存位置和文件名格式

  - 修改`/proc/sys/kernel/core_pattern`

  > `# echo /tmp/core-%e-%p-%t > /proc/sys/kernel/core_pattern`[^3]

### 使用 gdb 调试核心转储文件

Core dump 对于编程人员诊断和调试程序是非常有帮助的，因为对于有些程序错误是很难重现的，例如指针异常，而 Core dump 文件可以再现程序出错时的情景。我们使用 Linux 平台上的 `gdb` 程序来调试 core 文件，一般步骤如下：

1. 编译源文件：`gcc test.c -o test -g `
2. 打开core dump（`ulimit -c unlimited`）使程序异常终止时能生成 core 文件
3. 运行程序后使用命令`gdb [filename] [core file] -q`[^4]查看 core 文件

### 示例

```shell
root@4found:~/core_test# cat test.c
#include<stdio.h>
int main(int argc,char **argv)
{
        char buf[5];
        scanf("%s",buf);
        return 0;
}
root@4found:~/core_test# gcc test.c -o test -g
root@4found:~/core_test# ./test
aaaaaaa
Segmentation fault (core dumped)
root@4found:~/core_test# ll
total 112
-rw------- 1 root root 360448 Sep 13 14:37 core-test-5106-1568356621
-rwxr-xr-x 1 root root  17816 Sep 13 14:36 test
-rw-r--r-- 1 root root     96 Sep 13 14:24 test.c
root@4found:~/core_test# file core-test-5106-1568356621
core-test-5106-1568356621: ELF 32-bit LSB core file, Intel 80386, version 1 (SYSV), SVR4-style, from './test', real uid: 0, effective uid: 0, real gid: 0, effective gid: 0, execfn: './test', platform: 'i686'
root@4found:~/core_test# gdb test core-test-5106-1568356621 -q
Reading symbols from test...done.
[New LWP 5106]
...
Core was generated by `./test'.
Program terminated with signal SIGSEGV, Segmentation fault.
#0  0x0047b1db in main (argc=<error reading variable: Cannot access memory at address 0xbf006161>,
    argv=<error reading variable: Cannot access memory at address 0xbf006165>) at test.c:7
7       }
(gdb) info frame
Stack level 0, frame at 0xbf006161:
 eip = 0x47b1db in main (test.c:7); saved eip = <not saved>
 Outermost frame: Cannot access memory at address 0xbf00615d
 source language c.
 Arglist at unknown address.
 Locals at unknown address, Previous frame's sp is 0xbf006161
Cannot access memory at address 0xbf00615d
(gdb) quit
```



### Refer

- [ctf-all-in-one](https://github.com/firmianay/CTF-All-In-One)
- [man5_core](http://man7.org/linux/man-pages/man5/core.5.html)

---

[^1]: 当程序运行的过程中异常终止或崩溃，操作系统会将程序当时的内存、寄存器状态、堆栈指针、内存管理信息等记录下来，保存在一个文件中，这种行为就叫做核心转储（Core Dump）
[^2]: 其中`pid `表示该进程的 PID
[^3]: 此时生成的文件保存在` /tmp/` 目录下，文件名格式为 `core-[filename]-[pid]-[time]`
[^4]: `filename`为可执行程序名，`core file`为生成的 core 文件名