###### **`for`**命令[^1]

```bash
#!/bin/zsh
for test in Alabama Alaska Arkansas California ; do
	echo "The next state is $test"
done
# ------------------
# for var in list 
# do
# 	commands
# done
# ------------------
# for var in list; do
```

```bash
#!/bin/zsh
# --------it's wrong output-------
# for test in I don't know if this'll work ; do
#	echo "word: $test"
# done
# --------it's right output-------
for test in I don\'t know if this\'ll work ; do
	echo "word: $test"
done
# --------it's right output-------
for test in I don\'t know if "this'll" work ; do
	echo "word: $test"
done
```

```bash
#!/bin/zsh
for test in Nevada "New Hampshire" "New Mexico" "New York"
do
    echo "Now going to $test"
done
```

###### 从变量读取列表 [^2]

```bash
#!/bin/zsh
list="Alabama Alaska Arizona Arkansas Colorado"
list=$list" Connecticut"
for state in $list ; do
	echo "Have you ever visited $state ?"
done
```

###### 从命令读取值

```bash
#!/bin/zsh
file="./test/wes.txt"
#相对路径
for state in $(cat $file); do
	echo "Visit beautiful $state"
done
```

###### 更改字段分隔符 [^3]

```bash
#!/bin/zsh
file="./test/wes.txt"
IFS=$'\n'
# IFS=$' \t\n'
# IFS=:
# IFS=$'\n':;"
# 上面这个赋值会将换行符、冒号、分号和双引号作为字段分隔符
for state in $(cat $file);do
	echo "Visit beautiful $state"
done
```

```bash
# 在处理代码量较大的脚本时，可能在一个地方需要修改IFS的值，然后忽略这次修改，在脚本的其他地方继续沿用IFS的默认值
IFS.OLD=$IFS 
IFS=$'\n' 
<在代码中使用新的IFS值> 
IFS=$IFS.OLD
<之后使用原来的IFS值> 
```

###### 用通配符读取目录

```bash
#!/bin/zsh
for file in $PWD/* ; do
	if [ -d $file ]; then
		echo "$file is a directory"
	elif [ -f $file ]; then
		echo "$file is a file"
	fi
done
#------------------------
# 为确保脚本遇到包含空格的目录名和文件名（合法）不会出错，可以将$file变量用双引号圈起来
# if [-d "$file"] ; then
```

```bash
#!/bin/zsh
for file in $PWD/* $PWD/.* $PWD/adfg; do
	if [ -d "$file" ]; then
		echo "$file is a directory"
	elif [ -f "$file" ]; then
		echo "$file is a file"
	else
		echo "$file doesn't exist"
	fi
done
```

###### C 语言风格的**`for`** 命令

```bash
#!/bin/zsh
for (( i = 0; i < 10; i++ )); do
	echo "The num is $i"
done
```

```bash
#!/bin/zsh
for (( a = 1, b = 10; a <= 10; a++, b-- )); do
	ans=$(echo "$a - $b" | bc)
	echo "$a - $b = $ans"
done
#------output--------
# 1 - 10 = -9
# 2 - 9 = -7
# 3 - 8 = -5
# ...
```

###### **`while`**命令 [^4]

```bash
#!/bin/zsh
var1=10
while [[ $var1 -gt 0 ]]; do
	echo $var1
	var1=$[ $var1 - 1 ]
done
```

```bash
#!/bin/zsh
var1=10
while echo $var1 ; [ $var1 -ge 0 ];do
		echo "This is inside the loop"
		var1=$[ $var1 - 1 ]
done
```

###### **`until`**命令 [^5]

```bash
#!/bin/zsh
var1=100
until [[ $var1 -eq 0 ]]; do
	echo $var1
	var1=$[ $var1 - 25 ]
done
#-------output------
# 100
# 75
# 50
# 25
```

```bash
#!/bin/zsh
var1=100
until 	echo $var1 ; [[ $var1 -eq 0 ]]; do
	var1=$[ $var1 - 25 ]
done
#------output--------
# 100
# 75
# 50
# 25
# 0
```

###### 嵌套循环

```bash
#!/bin/zsh
for (( a = 1; a <= 3; a++ )); do
	echo "Starting loop $a:"
	for (( b = 0; b <= 3; b++ )); do
		echo "	Inside loop: $b"
	done
done
```

```bash
#!/bin/zsh
var1=5
while [[ $var1 -ge 0 ]]; do
	echo "Outer loop: $var1"
	for (( var2 = 1; var2 < 3; var2++ )); do
		var3=$[ $var1 * $var2 ]
		echo "	Inner loop: $var1 * $var2 = $var3 "
	done
	var1=$[ $var1 - 1 ]
done
```

```bash
#!/bin/zsh
var1=5
until [[ $var1 -lt 0 ]]; do
	echo "Outer loop: $var1"
	for (( var2 = 1; var2 < 3; var2++ )); do
		var3=$[ $var1 * $var2 ]
		echo "	Inner loop: $var1 * $var2 = $var3"
	done
	var1=$[ $var1 - 1 ]
done
```

```bash
#!/bin/zsh
var1=5
until [[ $var1 -lt 0 ]]; do
	echo "Outer loop: $var1"
	var2=1
	while [[ $var2 -lt 3 ]]; do
		var3=$[ $var1 * $var2 ]
		echo "	Inner loop: $var1 * $var2 = $var3"
		var2=$[ $var2 + 1 ]
	done
	var1=$[ $var1 - 1 ]
done
```

###### 循环处理文件数据

```bash
#!/bin/bash
# IFS.OLD=$IFS
IFS=$'\n'
for entry in $(cat /etc/passwd | grep -v "^#\|^_"); do
	echo "Values in $entry -"
	IFS=:
	for value in $entry; do
		echo "	$value"
	done
done
#----------注意------------
# shell 是 bash 可用
# 匹配字段若存在‘*‘，则会运行“echo *”
```

###### **`break`**命令 [^6]

```bash
#!/bin/bash
for var1 in 1 2 3 4 5 6 7 8;do
	if [[ $var1 -eq 5 ]]; then
		break
	fi
	echo "Iteration number: $var1"
done
echo "The for loop is completed"
```

```bash
#!/bin/bash
# break 跳出单个循环
var1=1
while [[ $var1 -lt 10 ]]; do
	if [[ $var1 -eq 5 ]]; then
		break
	fi
	echo "Iteration: $var1"
	var1=$[ $var1 + 1 ]
done
echo "The while loop is completed"
```

```bash
#!/bin/bash
#  break 跳出内部循环
var1=1
until [[ $var1 -ge 4 ]]; do
	echo "Outer loop: $var1"
	for var2 in 1 2 3 4 5 6 7 8; do
		if [[ $var2 -ge 5 ]]; then
			break
		fi
		echo "	Inner loop: $var2"
	done
	var1=$[ $var1 + 1 ]
done
```

```bash
#!/bin/bash
# break 跳出外部循环
# break命令接受单个命令行参数值: n
# 其中n指定了要跳出的循环层级。默认情况下，n为1，表明跳出的是当前的循环。如果你将n设为2，break命令就会停止下一级的外部循环

for (( i = 0; i < 4; i++ )); do
	echo "Outer loop: $i"
	for (( j = 0; j < 100; j++ )); do
		if [[ $j -gt 4 ]]; then
			break 2	
		fi
		echo "	Inner loop: $j"
	done
done
```

###### **`continue`**命令 [^7]

```bash
#!/bin/bash
for (( i = 0; i < 15; i++ )); do
	if [[ $i -gt 5 ]] && [[ $i -lt 10 ]]; then
		continue	
	fi
	echo "Iteration number: $i"
done
```

###### 处理循环的输出

```bash
#!/bin/bash
for state in "North Dakota" Connecticut Illinote Alabama; do
	echo "$state is the next place to go"
done|sort >> ou3tput.txt
echo "This completes our travels"
```

###### 示例

```bash
#!/bin/bash
# 查看本目录下的可执行文件
for file in $PWD/*; do
	if [[ -x $file ]] && ! [[ -d $file ]]; then
		echo "$file could be run"
	# else
	# 	echo "$file couldn't be run"
	fi
done>ou3tput.txt
echo "operation done."
```

###### 处理用户输入

```bash
#!/bin/bash
total=$[ $1 * $2 ]
echo The first parameter is $1.
echo The second parameter is $2.
echo The total value is $total
```

```bash
#!/bin/bash
echo "Hello $1 , nice to meet you"
#-----input-----
# $ ./test 'John smith'
# 每个参数都是用空格分隔的，所以shell会将空格当成两个值的分隔符。要在参数值中包含空格，必须要用引号(单引号或双引号均可)
```

```bash
#!/bin/bash
total=$[ ${10} * ${11} ]
echo the tenth parameter is ${10}.
echo the eleventh parameter is ${11}.
echo the total is $total

#如果脚本需要的命令行参数不止9个，你仍然可以处理，但是需要稍微修改一下变量名。在第9个变量之后，你必须在变量数字周围加上花括号，比如${10}
```

```bash
#!/bin/zsh
name=$(basename $0)
echo the zero parameter is $name.

# basename命令会返回不包含路径的脚本名

##!/bin/zsh
#for file in $PWD/*; do
#	if [[ -e $file ]]; then
#		echo "$(basename $file) is a file"
#	fi
#done
```

```bash
#!/bin/zsh
name=$(basename $0)
if [[ $name = "addem" ]]; then
	total=$[ $1+$2 ]
elif [[ $name = "multem" ]]; then
	total=$[ $1*$2 ]
fi
echo The calculated valued is $total
#创建了两个不同的文件名:一个通过复制文件(cp test5.sh addem)创建(addem)，另一 个通过链接(ln -s test5.sh multem)创建(multem)。在两种情况下都会先获得脚本的基本名称，然后根据该值执行相应的功能。
```

###### 测试参数

```bash
#!/bin/zsh
if [[ -n $1 ]]; then
	echo Hello $1, glad to meet you.
else
	echo "Sorry, you did not identify yourself."
fi

# 在使用参数前一定要检查其中是否存在数据
```

```bash
#!/bin/zsh
echo There are $# parameters supplied.

#命令行参数的个数统计,可以在脚本中任何地方使用这个特殊变量，就跟普通变量一样
```

```bash
#!/bin/zsh
name=$(basename $0)
if [[ $# -ne 2 ]]; then
	echo Usage: $name a b
else
	total=$[ $1+$2 ]
	echo The total is $total
fi
```

```bash
#!/bin/bash
params=$#
echo the last parameter was $params
echo the last parameter was ${!#}

#------注意-------
# 这里貌似 shell 为 bash 才可用${!#}求出最后的一个参数，比如 zsh shell 就不行
# 重要的是要注意，当命令行上没有任何参数时，$#的值为0， params变量的值也一样，但${!#}变量会返回命令行用到的脚本名
```

###### 抓取所有的数据 [^8]

```bash
#!/bin/bash
echo "Using the \$* method: $*"
echo "Using the \$@ method: $@"
```

```bash
#!/bin/zsh
count=1
for param in "$*"; do
	echo "\$* parameter #$count = $param"
	count=$[ $count + 1 ]
done
echo
count=1
for param in "$@"; do
	echo "\$@ parameter #$count = $param"
	count=$[ $count + 1 ]
done

#------input-------
# /test3 2 tew qe 23
#------output------
# $* parameter #1 = 2 tew qe 23
#  
# $@ parameter #1 = 2
# $@ parameter #2 = tew
# $@ parameter #3 = qe
# $@ parameter #4 = 23
```

[下一篇](shell3.md)

---

[^1]: 每次`for`命令遍历值列表，它都会将列表中的下个值赋给`test`变量。`test`变量可以像`for` 命令语句中的其他脚本变量一样使用。在最后一次迭代后，`test`变量的值会在shell脚本的剩余部分一直保持有效。==它会一直保持最后一次迭代的值(除非你修改了它)==。`for`命令用==空格==来划分列表中的每个值。如果在单独的数据值中有空格，就必须用==双引号==将这些值圈起来
[^2]: 这种方法zsh不可用,与 shell 类型有关，改为 bash shell 就可以
[^3]: 造成这个问题的原因是特殊的环境变量`IFS`，叫作==内部字段分隔符==(internal field separator)。 `IFS`环境变量定义了bash shell用作字段分隔符的一系列字符。通过`set | grep IFS` 查看本环境下的 `IFS`
[^4]: `while`命令允许你在`while`语句行定义多个测试命令。只有==最后一个==测试命令的退出状态码会被用来决定什么时候结束循环
[^5]: `until`命令和`while`命令工作的方式完全相反。只有测试命令的退出状态码==不为0==，bash shell才会执行循环中列出的命令。 一旦测试命令返回了退出状态码0，循环就结束了;只有==最后一个==命令的退出状态码决定了bash shell是否执行结束
[^6]: 可以用`break`命令来退出任意类型的循环，包括` while`和`until`循环
[^7]: `continue`命令可以==提前中止某次循环==中的命令，但并不会完全终止整个循环。可以在循环内部设置shell不执行命令的条件
[^8]: `$*`变量会将命令行上提供的所有参数当作一个单词保存。这个单词包含了命令行中出现的每 一个参数值。基本上`$*`变量会将这些参数视为==一个整体==，而不是多个个体。另一方面，​`$@`变量会将命令行上提供的所有参数当作==同一字符串中的多个独立的单词==。这样 你就能够遍历所有的参数值，得到每个参数。这通常通过`for`命令完成。

