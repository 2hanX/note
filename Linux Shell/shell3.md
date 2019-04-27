###### 移动变量 [^1]

```bash
#!/bin/bash
count=1
while [[ -n $1 ]]; do
	echo "Parameter #$count = $1"
	count=$[ $count + 1]
	shift
done
```

```bash
#!/bin/bash
echo "The original parameters: $*"
shift 2
echo "Here's the new first parameter: $1"

#------input-----
#  ./test2 2 5 4 2
#-----output-----
# The original parameters: 2 5 4 2
# Here's the new first parameter: 4
```

###### 查找选项

```bash
#!/bin/bash
# 处理简单选项
while [[ -n "$1" ]]; do
	case "$1" in
		-a )	echo "Found the -a option" ;;
		-b )	echo "Found the -b option" ;;
		-c )	echo "Found the -c option" ;;	
		*  )	echo "$1 is not an opions" ;;
	esac
	shift
done
```

```bash
#!/bin/bash
# 分离参数和选项
while [[ -n "$1" ]]; do
	case "$1" in
		-a )	echo "Found the -a option" ;;
		-b )	echo "Found the -b option" ;;
		-c )	echo "Found the -c option" ;;
		-- )	shift
				break
				;;
		* )		echo "$1 is not an opions"	;;
	esac
	shift 
done

count=1
for param in $@; do
	echo "Parameter #count: $param"
	count=$[ $count + 1 ]
done

#----input-------
# ./test16.sh -c -a -t test1 -- test1 test2 
#-----output-----
#	Found the -c option
#   Found the -a option
#   -t is not an option
#   test1 is not an option
#   Parameter #1: test1
#   Parameter #2: test2

#-------注意-------
# 在遇到双破折线时，脚本用`break`命令来跳出`while`循环。由于过早地跳出了循环，我们需要再加一条`shift`命令来将双破折线移出参数变量
# shell会用双破折线来表明选项列表结束。在双破折线之后，脚本就可以放心地将剩下的命令行参数当作参数，而不是选项来处理了
```

```bash
#!/bin/bash
# 处理带值的选项(区分参数和选项)
while [[ -n "$1" ]]; do
	case "$1" in
		-a )	echo "Found the -a option"	
				;;
		-b )	param="$2"
				echo "Found the -b option,with parameter value $param"
				shift
				;;
		-c )	echo "Found the -c option"
				;;
		-- )	shift
				break
				;;
		*  )	echo "$1 is not an option"
				;;
	esac
	shift
done

count=1
for param in "$@"; do
	echo "parameter #count : $param"
	count=$[ $count + 1 ]
done

#----input-------
# ./test2 -a -b 32gs -t -- test1 
#-----output-----
# Found the -a option
# Found the -b option,with parameter value 32gs
# -t is not an option
# parameter #count : test1
```

###### 使用**`getopt`**命令 [^2]

```bash
# getopt optstring parameters

getopt -q ab:cd -a -b test1 -cd test2 test3

#冒号(:)被放在了字母b后面，因为b选项需要一个参数值，在自动转换后
# -q 忽略错误信息，现在貌似取消了这个选项

-a -b test1 -c -d -- test2 test3
```

```bash
#!/bin/bash
set -- $(getopt  ab:cd "$@")
# set命令的选项之一是双破折线(--)，它会将命令行参数替换成set命令的命令行值。 然后，该方法会将原始脚本的命令行参数传给getopt命令，之后再将getopt命令的输出传给set命令，用getopt格式化后的命令行参数来替换原始的命令行参数
#-------提醒--------
# getopt命令并不擅长处理带空格和引号的参数值。它会将空格当作参数分隔符，而不是根据双引号将二者当作一个参数
while [[ -n "$1" ]]; do
	case "$1" in
		-a )	echo "Found the -a option"	
				;;
		-b )	param="$2"
				echo "Found the -b option,with parameter value $param"
				shift
				;;
		-c )	echo "Found the -c option"
				;;
		-- )	shift
				break
				;;
		*  )	echo "$1 is not an option"
				;;
	esac
	shift
done

count=1
for param in "$@"; do
	echo "parameter #$count : $param"
	count=$[ $count + 1 ]
done
```

###### 更高级的**`getopts`** [^3]

```bash
#!/bin/bash
# getopts optstring variable
while  getopts :ab:c opt ; do
	case "$opt" in
		a ) echo "Found the -a option" ;;
		b ) echo "Found the -b option,with value $OPTARG" ;;
		c ) echo "Found the -c option" ;;
		* ) echo "Unknown option: $opt" ;;
	esac
done
#-----input-----
# ./test19.sh -b "test1 test2" -a
# ./test19.sh -abtest1

# getopts处理每个选项时，它会将OPTIND环境变量值增一。在getopts完成处理时，你可以使用shift命令和OPTIND值来移动参数
```

###### 将选项标准化 

常用的Linux命令选项

| 选项 | 描述                               |
| ---- | ---------------------------------- |
| `-a` | 显示所有对象                       |
| `-c` | 生成一个计数                       |
| `-d` | 指定一个目录                       |
| `-e` | 扩展一个对象                       |
| `-f` | 指定读入数据的文件                 |
| `-h` | 显示命令的帮助信息                 |
| `-i` | 忽略文本大小写                     |
| `-l` | 产生输出的长格式版本               |
| `-n` | 使用非交互模式(批处理)             |
| `-o` | 将所有输出重定向到的指定的输出文件 |
| `-q` | 以安静模式运行                     |
| `-r` | 递归地处理目录和文件               |
| `-s` | 以安静模式运行                     |
| `-v` | 生成详细输出                       |
| `-x` | 排除某个对象                       |
| `-y` | 对所有问题回答 yes                 |

###### 获得用户输入

```bash
#!/bin/bash
echo -n "Enter your name: "
read name
echo "Hello $name, welcome to my world."

# read命令会将姓和名保存在同一个变量中。 read命令会将提示符后输入的所有数据分配给单个变量，要么你就指定多个变量。输入的每个数据值都会分配给变量列表中的下一个变量。如果变量数量不够，剩下的数据就全部分配给最后一个变量
```

```bash
#!/bin/bash
read -p "Please enter your age: " age
days=$[ $age * 365 ]
echo "That makes you over $days days old! "
```

```bash
#!/bin/bash
read -p "Enter your name: "
echo
echo Hello $REPLY, welcome to my program.

# REPLY环境变量会保存输入的所有数据，可以在shell脚本中像其他变量一样使用
```

###### 超时 [^4]

```bash
#!/bin/bash
if read -t 5 -p "Please enter your name: " name ; then
	echo "Hello $name, welcome to my world"
else
	echo 
	echo "Sorry, too slow! "
fi
```

```bash
#!/bin/bash
read -n1 -p "Do you want to continue [Y/N]? " answer
case $answer in
	Y|y )	echo
			echo "fine, continue on ... ";;
	N|n )	echo
			echo "OK, goodbye"
			exit ;;
esac
echo "This is the end of the script"

# -n选项和值1一起使用，告诉read命令在接受单个字符后退出。只要按下单个字符回答后，read命令就会接受输入并将它传给变量，无需按回车键
```

###### 隐藏方式读取

```bash
#!/bin/bash
read -s -p "Enter your password: " pass
echo
echo "Is your password really $pass? "

# `-s`选项可以避免在read命令中输入的数据出现在显示器上(实际上，数据会被显示，只是 read命令会将文本颜色设成跟背景色一样)
```

###### 从文件中读取

```bash
#!/bin/bash
count=1
cat ou3tput.txt | while read line ; do
	echo "Line $count: $line"
	count=$[ $count + 1 ]
done
echo "Finished processing the file"

# 每次调用read命令，它都会从文件中读取一行文本。当文件中再没有内容时，read命令会退出并返回非零退出状态码
```

[下一篇](shell4.md)

---

[^1]: 在使用`shift`命令时，默认情况下它会将每个参数变量==向左==移动一个位置。所以，变量`$3`的值会移到`$2`中，变量`$2`的值会移到`$1`中，而变量`$1`的值则会被==删除==(注意，变量`$0`的值，也就是程序名，不会改变)。
[^2]: `getopt`命令是一个在处理命令行选项和参数时非常方便的工具。它能够识别命令行参数， 从而在脚本中解析它们时更方便;`getopt`命令可以接受一系列==任意形式的命令行选项和参数==，并==自动==将它们转换成适当的格 式。它的命令格式如下:`getopt optstring parameters`
[^3]: 比 `getopt`多了一些 扩展功能，此外前者将命令行上选项和参数处理后==只生成一个输出==，而`getopts`命令能够和已有的shell参数变量配合默契，即每次调用它时，它一次只处理命令行上检测到的==一个参数==。处理完所有的参数后，它会退出并返回一个大于`0`的退出状态码
[^4]: `-t`选项指定了`read`命令等待输入的秒数。当计时器过期后，`read`命令会返回一个非零退出状态码