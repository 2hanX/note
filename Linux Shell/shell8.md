#### 初识sed和gawk

###### `sed `编辑器[^1]

`sed options script file`

| 选项        | 描述                                                 |
| ----------- | ---------------------------------------------------- |
| `-e script` | 在处理输入时，将script中指定的命令添加到已有的命令中 |
| `-f file`   | 在处理输入时，将file中指定的命令添加到已有的命令中   |
| `-n`        | 不产生命令输出，使用print命令来完成输出              |

`echo "This is a test" | sed 's/test/big test'`

`sed 's/dog/cat/' file.txt`

###### 在命令行使用多个编辑器命令

`sed -e 's/brown/green/; s/dog/cat/' file.txt`

###### 从文件中读取编辑器命令

`sed -f script1.sed data1.txt`

###### `gawk` 程序

`gawk options program file`

| 选项           | 描述                               |
| -------------- | ---------------------------------- |
| `-F fs`        | 指定行中划分数据字段的字段分隔符   |
| `-f file`      | 从指定的文件中读取程序             |
| `-v var=value` | 定义gawk程序中的一个变量及其默认值 |
| `-mf N`        | 指定要处理的数据文件中的最大字段数 |
| `-mr N`        | 指定数据文件中的最大数据行数       |
| `-W keyword`   | 指定gawk的兼容模式或警告等级       |

###### 从命令行读取程序脚本

`gawk '{print "Hello World!"}'` [^3]

###### 使用数据字段变量 [^2]

`gawk '{print $1}'  file.txt `

`gawk -F: '{print $1}' /etc/passwd`

###### 在程序脚本中使用多个命令

`echo "My name is Rich" | gawk '{$4="Christine"; print $0}'`

###### 从文件中读取程序

```cmd
$ cat script2.gawk
{print $1 "'s home directory is " $6}
$
$ gawk -F: -f script2.gawk /etc/passwd
```

###### 在处理数据前运行脚本 [^4]

`gawk 'BEGIN {print "Hello World!"}'`

`gawk 'BEGIN {print "The data3 File Contents:"}{print $0}' data3.txt`

###### 在处理数据后运行脚本

` gawk 'BEGIN {print "The data3 File Contents:"}{print $0} END {print "End of File"}' data3.txt`

```tiki wiki
#script4.gawk
BEGIN {
	print "The latest list of users and shells"
	print "UserID \t Shell"
	print "-------\t-------"
	FS=":"
}
{
	print $1"       \t  "$7
}

END {
	print "This concludes the listing"
}
```

`grep -v '^#' /etc/passwd | gawk -f script4.gawk`

###### `sed `编辑器基础

`sed 's/test/trial/' data.txt` [^5]

`sed 's/test/trial/2' data.txt`

`sed 's/test/trial/g' data.txt`

`sed -n 's/test/trial/p' data.txt`

`sed 's/test/trial/w test.txt' data.txt`

###### 替换字符

`sed 's!/bin/bash!/bin/zsh/!' /etc/passwd`

###### 使用地址 [^6]

`sed '2s/dog/cat/' data.txt`

`sed '2,5s/of/to/g' file.txt`

`sed '2,$s/of/to/g' file.txt`

`sed '/root/s/bash/csh/' /etc/passwd`

```shell
$ sed '2{
> s/fox/elephant/
> s/dog/cat/
> }' data1.txt
```

###### 删除行

`sed '3d' data.txt`

`sed '2,3d' data.txt`

`sed '2,$d' data.txt`

`sed '/dog/d' data.txt`

`sed '/dog2/,/dog4/d' lsof.txt` [^7]

###### 插入和附加文本 [^8]

```shell
$ echo "Test Line 2" | sed 'i\
> Test Line 1'
```

```shell
$ echo "Test Line 2" | sed 'a\
> Test Line 1'`
```

```shell
$ sed '3i\
> This is an inserted line.' data6.txt
```

```shell
$ sed '$a\
> this is an end line.' data6.txt
```

```shell
$ sed '1i\
> this is the first line.' data6.txt
```

###### 修改行

```shell
$ sed '3c\
> This is a changed line of text.' data6.txt
```

```shell
$ sed '/number 3/c\
> This is a changed line of text.' data6.txt
```

###### 转换命令 [^9]

`sed 'y/123/789/' data8.txt`

`echo "This 1 is a test of 1 try." | sed 'y/123/456/'`

###### 回顾打印 [^11]

`echo "this is a test" | sed 'p'`

`sed -n '/number 3/p' data.txt` [^10]

`sed -n '2,3p' data.txt`

```shell
$ sed -n '/3/{
> p
> s/line/test/p
> }' data.txt
```

`sed '=' data.txt`

```shell
$ sed -n '/number 4/{
> =
> p
> }' data.txt
```

`sed -n 'l' data.txt `[^12]

###### 写入文件 [^13]

`sed '1,2w test.txt' data.txt` [^14]

`sed -n '/Browncoat/w Browncoat.txt' data.txt`

###### 从文件读取数据 [^15]

`sed '3r test.txt' data.txt`

`sed '/number 2/r test.txt' data.txt`

`sed '$r test.txt' data.txt`

```shell
$ sed '/LIST/{
> r file.txt
> d
> }' data.txt
# 利用另一个文件中的数据来替换文件中的占位文本
```

[下一篇](shell9.md)

---

[^1]: `sed`编辑器并不会修改文本文件的数据。它只会将修改后的数据发送到STDOUT。如果你查看原来的文本文件，它仍然保留着原始数据。
[^2]: `gawk`的主要特性之一是其处理文本文件中数据的能力。它会自动给一行中的每个数据元素分配一个变量。默认情况下，`gawk`会将如下变量分配给它在文本行中发现的数据字段:`$0`代表整个文本行;`$1`代表文本行中的第1个数据字段;`$n`代表文本行中的第n个数据字段。
[^3]: 因为没有在命令行中指定文件名，`gawk`程序会从STDIN中获得数据。当运行这个程序的时候，它会等着读取来自STDIN的文本。要退出程序， 只需按下`Ctrl+D`组合键来表明数据结束。
[^4]: 默认情况下，`gawk`会从输入中读取一行文本，然后针 对该行的数据执行程序脚本。有时可能需要在处理数据前运行脚本，比如为报告创建标题。`BEGIN` 关键字就是用来做这个的。它会强制gawk在读取数据前执行BEGIN关键字后指定的程序脚本
[^5]: 替换命令在替换多行中的文本时能正常工作，但默认情况下它只替换==每行中出现的第一处==。 要让替换命令能够替换一行中不同地方出现的文本必须使用替换标记(substitution flag)。替换标记会在替换命令字符串之后设置。`s/pattern/replacement/flags `。 有4种可用的替换标记:   `数字`，表明新文本将替换第几处模式匹配的地方; `g`，表明新文本将会替换所有匹配的文本;  `p`，表明原先行的内容要打印出来; `w file`，将替换的结果写到文件中。
[^6]:  默认情况下，在`sed`编辑器中使用的命令会作用于文本数据的所有行。如果只想将命令作用于==特定行或某些行==，则必须用行寻址(line addressing)。
[^7]:  也可以使用两个文本模式来删除某个区间内的行，但这么做时要小心。你指定的第一个模式会“打开”行删除功能，第二个模式会“关闭”行删除功能。`sed`编辑器会删除两个指定行之间的==所有行(包括指定的行)==
[^8]: 插入(insert)命令`(i)`会在指定行前增加一个新行;  附加(append)命令(`a`)会在指定行后增加一个新行;它们==不能在单个命令行上使用==。你必须指定是要将行插入还是附加到另一行。
[^9]: 转换(transform)命令(`y`)是唯一可以处理单个字符的`sed`编辑器命令。转换命令格式 如下。`[address]y/inchars/outchars/` 。转换命令会对`inchars`和`outchars`值进行一对一的映射。`inchars`中的第一个字符会被转换为`outchars`中的第一个字符，第二个字符会被转换成`outchars`中的第二个字符。这个映射过程会一直持续到处理完指定字符。==如果`inchars`和`outchars`的长度不同，则`sed`编辑器会产生一条错误消息==
[^10]: `-n`选项，禁止输出其他行，只打印包含匹配文本模式的行
[^11]: `p`命令用来打印文本行;  `等号(=)`命令用来打印行号; `l(小写的L)`命令用来列出行。
[^12]: `列出(list)命令(l)`可以打印数据流中的==文本和不可打印的ASCII字符==。任何不可打印 字符要么在其八进制值前加一个反斜线，要么使用标准C风格的命名法(用于常见的不可打印字符)，比如`\t`，来代表制表符。
[^13]: `[address]w filename`
[^14]: 将 data.txt数据流中的前两行打印到一个文本文件（test.txt）中 
[^15]: `[address]r filename`