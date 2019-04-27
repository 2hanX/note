###### 标准文件描述符 [^1]

| 文件描述符 | 缩写   | 描述     |
| ---------- | ------ | -------- |
| 0          | STDIN  | 标准输入 |
| 1          | STDOUT | 标准输出 |
| 2          | STDERR | 标准错误 |

###### 重定向错误

`ls -al badfile 2> test4`

###### 重定向错误和数据 [^2]

`ls -al test test2 badtest 2> test6 1> test7`

`ls -al test test2 test3 badtest &> test7` [^3]

###### 在脚本中重定向输出 [^4]

```bash
#!/bin/bash
# 临时重定向
echo "This is an error" >&2
echo "This is normal output"

#----input----
# ./test2 2> output.txt
```

```bash
#!/bin/bash
# 永久重定向
exec 1> output.txt
echo "This is a test of redirecting all output"
echo "from a script to another file."
```

```bash
#!/bin/bash
exec 2> testerror.txt
echo "This is the start of the script"
echo "now redirecting all output to another location"
exec 1> testout.txt
echo "This output should go to the testout file"
echo "but this should go to the testerror file" >&2
```

###### 在脚本中重定向输入

```bash
#!/bin/bash
exec 0< input.txt
count=1
while  read line ; do
	echo "Line #count : $line"
	count=$[ $count+1 ]
done
```

###### 创建自己的重定向 [^5]

```bash
#!/bin/bash
# 创建输出文件描述符
exec 3> testout.txt
echo "This should display on the monitor"
echo "and this should be stored in the file" >&3
echo "Then this should be back on the monitor"
```

```bash
#!/bin/bash
# 备份或恢复重定向文件标识符
exec 3>&1
exec 1> testout.txt
echo "This should store in the output file"
echo "along with this line."
exec 1>&3
echo "Now things should be back to normal"
```

###### 创建输入文件描述符

```bash
#!/bin/bash
exec 6<&0
exec 0< testin.txt
count=1
while read line;do
	echo "Line #$count: $line"
	count=$[$count + 1]
done
exec 0<&6
read -p "Are you done now? " answer
case $answer in
	Y|y )	echo "Good";;
	N|n)	echo "Sorry, this is the end."
esac
```

###### 创建读写文件描述符

```bash
#!/bin/bash
exec 3<> test.txt
read line <&3
echo "Read: $line"
echo "This is a test line" >&3

#----cat test.txt----
# This is a first line
# This is a second line
# This is a third line
#----output----
# This is a first line
# This is a test line
# e
# This is a third line
#------注意------
# shell会维护一个内部指针，指明在文件中的当前位置。任何读或写都会从文件指针上次的位置开始
# 当脚本向文件中写入数据时，它会从文件指针所处的位置开始。`read`命令读取了第一行数据，所以它使得文件指针指向了第二行数据的第一个字符。在`echo`语句将数据输出到文件时，它会将数据放在文件指针的当前位置，覆盖了该位置的已有数据
```

###### 关闭文件描述符

```bash
#!/bin/bash
exec 3> testout.txt
echo "This is a test line of data" >&3
exec 3>&-
echo "This won't work" >&3
```

```bash
#!/bin/bash
exec 3> testout.txt
echo "This is a test line of data" >&3
exec 3>&-
cat testout.txt
exec 3> testout.txt
echo "This'll be bad" >&3

# 在关闭文件描述符时还要注意。如果随后你在脚本中打开了同一个输出文件，shell 会用一个新文件来替换已有文件。这意味着如果你输出数据，它就会覆盖已有文件
```

###### 列出打开的文件描述符 [^6]

`/usr/sbin/lsof -a -p $$ -d 0,1,2` [^7]

| 列      | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| COMMAND | 正在运行的命令名的前9个字符                                  |
| PID     | 进程的PID                                                    |
| USER    | 进程属主的登录名                                             |
| FD      | 文件描述符号以及访问类型(r代表读，w代表写，u代表读写)        |
| TYPE    | 文件的类型(CHR代表字符型，BLK代表块型，DIR代表目录，REG代表常规文件) |
| DEVICE  | 设备的设备号(主设备号和从设备号)                             |
| SIZE    | 如果有的话，表示文件的大小                                   |
| NODE    | 本地文件的节点号                                             |
| NAME    | 文件名                                                       |

```bash
#!/bin/bash
# 查看打开了多个替代性文件描述符的脚本中使用lsof命令的结果
exec 3> testout.txt
exec 6> testerror.txt
exec 7< testin.txt

/usr/sbin/lsof -a -p $$ -d0,1,2,3,6,7 > lsof.txt
```

###### 阻止命令输出

`ls -al > /dev/null`

` ls -al badfile test16 2> /dev/null` 

`cat /dev/null > testfile` [^8]

###### 创建临时文件 [^9]

`mktemp testing.XXXXXX` [^10]

```bash
#!/bin/bash
tempfile=$(mktemp test.XXXX)
exec 3> $tempfile
echo "This script writes to temp file $tempfile"
echo "This is the first line" >&3
echo "This is the second line" >&3
echo "This is the third line" >&3
echo "Done creating temp file. The contents are:"
cat $tempfile
rm -f $tempfile 2> /dev/null
```

###### 在 `/tmp` 目录创建临时文件 [^11]

`mktemp -t test.XXXX`

`mktemp -d dir.XXXX`

###### 记录消息 [^12]

`date | tee datafile` 

`date | tee -a datafile`

###### 实例

```bash
#!/bin/bash
outfile="members.sql"
IFS=','
while read lname address city state zip; do
	cat >> $outfile << EOF
	insert into members(lname,fname,address,city,state,zip)
	values('$lname','$fname','$address','$city','$state','$zip');
	EOF
done < ${1}
```

[下一篇](shell5.md)

---

[^1]: Linux系统将每个对象当作==文件处理==。这包括输入和输出进程。Linux用文件描述符(file descriptor)来标识每个文件对象。文件描述符是一个==非负整数==，可以唯一标识会话中打开 的文件。每个进程一次最多可以有九个文件描述符。出于特殊目的，bash shell保留了前三个文 件描述符(0、1和2)
[^2]: 如果想重定向错误和正常输出，必须用两个重定向符号。需要在符号前面放上待重定向数据所对应的文件描述符，然后指向用于保存数据的输出文件
[^3]: `&>`符号表示将`STDERR`和`STDOUT`的输出重定向到同一个输出文件
[^4]: 默认情况下，Linux会将`STDERR`导向`STDOUT`。但是，如果你在运行脚本时重定向了` STDERR`，脚本中所有导向`STDERR`的文本都会被重定向
[^5]: 在shell 中最多可以有==9==个打开的文件描述符。其他6个从3~8的文件描述符均可用作输入或输出重定向。 你可以将这些文件描述符中的任意一个分配给文件，然后在脚本中使用它们
[^6]: `lsof`命令会列出整个Linux系统打开的所有文件描述符;它会显示当前Linux系统上打开的每个文件的有关信息。这包括后台运行的所有进程以及登录到系统的任何用户
[^7]: `-p `指定进程ID(PID)，`-d`指定要显示的文件描述符编号。特殊环境变量`$$`(shell会将它设为当前PID)表示进程的当前PID。`-a`选项用来对其他两个选项的结果执行==布尔AND运算==
[^8]: 文件testfile仍然存在系统上，但现在它是空文件。这是==清除日志文件的一个常用方法==，因为 日志文件必须时刻准备等待应用程序操作
[^9]: 大多数Linux发行版配置了系统在启动时==自动删除== `/tmp`目录的所有文件。`mktemp`命令可以在`/tmp`目录中创建一个唯一的临时文件,但不用默认的`umask`值。它会将文件的读和写权限分配给文件的属主，并将你设成文件的属主。一旦创建了文件，你就在脚本中有了完整的读写权限， 但其他人没法访问它(root除外)
[^10]: `mktemp`命令会用6个字符码替换这6个X，从而保证文件名在目录中是唯一的;`X` 的个数自定
[^11]: `-t`选项会强制`mktemp`命令来在系统的临时目录来创建该文件。在用这个特性时，`mktemp`命令会返回用来创建临时文件的==全路径==，而不是只有文件名
[^12]:  `tee`命令相当于管道的一个==T==型接头。它将从STDIN过来的数据同时发往两处。一处是 STDOUT，另一处是`tee`命令行所指定的文件名;默认情况下，`tee`命令会在每次使用时==覆盖==输出文件内容,` -a`选项可以将数据追加到文件中

 