###### 创建函数 [^1]

```bash
function name { commands
}
```

```bash
#!/bin/bash
function func1 {
	echo "This is an example of a function"
}
count=1
while [[ $count -le 5 ]]; do
	func1
	count=$[ $count + 1 ]
done
echo "This is the end of the loop"
func1
echo "Now this is the end of the script"
```

###### 返回值 [^2]

```bash
#!/bin/bash
# 默认退出状态码
func1(){
	echo "trying to display a non-existent file"
	ls -l badfile
}
echo "testing the function: "
func1
echo "The exit status is: $?"

# 函数的退出状态码是1，这是因为函数中的`最后一条命令`没有成功运行。但你无法知道函数中其他命令中是否成功运行.也就是说函数中只要最后一条命令成功执行，不管其他命令是否成功执行，该函数的退出状态码依旧为0
```

```bash
#!/bin/bash
# 使用return命令
function db1 {
	read -p "Enter a value: " value
	echo "doubling the value"
	return $[ $value*2 ]
}
db1
echo "The new value is $?"

#-------注意---------
# 记住，函数一结束就取返回值; 退出状态码必须是0~255
```

```bash
#!/bin/bash
# 使用函数输出
function db1 {
	read -p "Enter a value: " value
	echo $[ $value*2 ]
}
result=$(db1)
echo "The new value is $result "
```

###### 在函数中使用变量

```bash
#!/bin/bash
function addem {
	if [[ $# -eq 0 ]] || [[ $# -gt 2 ]]; then
		echo -1
	elif [[ $# -eq 1 ]]; then
		echo $[ $1+$1 ]
	else
		echo $[ $1+$2 ]
	fi
}

echo -n "Adding 10 and 15: "
value=$(addem 10 15)
echo $value
echo -n "Let's try adding just one number: "
value=$(addem 10)
echo $value
echo -n "Now trying adding no numbers: "
value=$(addem)
echo $value
echo -n "Finally, try adding three numbers: "
value=$(addem 10 15 20)
echo $value

# 脚本中的addem函数首先会检查脚本传给它的参数数目。如果没有任何参数，或者参数多于两个，addem会返回值-1。如果只有一个参数，addem会将参数与自身相加。如果有两个参数，addem会将它们进行相加
```

```bash
#!/bin/bash
function func7 {
	echo $[ $1*$2 ]
}
if [[ $# -eq 2 ]]; then
	value=$(func7 $1 $2)
	echo "The result is $value"
else
	echo "Usage: badtest1 a b"
fi
```

###### 在函数中处理变量

```bash
#!/bin/bash
# 默认情况下，在脚本中定义的任何变量都是全局变量。在函数外定义的变量可在函数内正常访问
function func1 {
	temp=$[ $value + 5 ]
	result=$[ $temp*2 ]
}
temp=4
value=6
func1
echo "The result is $result"
if [[ $temp -gt $value ]]; then
	echo "temp is lager"
else
	echo "temp is smaller"
fi
#----output---
# The result is 22
# temp is lager
```

`local temp=$[ $value + 5 ]`

```bash
#!/bin/bash
function func1 {
	local temp=$[ $value + 5 ]
	result=$[ $temp*2 ]
}
temp=4
value=6
func1
echo "The result is $result"
if [[ $temp -gt $value ]]; then
	echo "temp is lager"
else
	echo "temp is smaller"
fi
#----output----
# The result is 22
# temp is smaller
```

###### 向函数传数组参数

```bash
#!/bin/bash
# 如果试图将该数组变量作为函数参数，函数只会取数组变量的第一个值
# 要解决这个问题，必须将该数组变量的值分解成单个的值，然后将这些值作为函数参数使用(如下一个代码段)
function testit {
	echo "the parameters are: $@"
	thisarray=$1
	echo "the received array is ${thisarray[*]}"
}
myarray=(1 2 3 4 5)
echo "The original array is: ${myarray[*]}"
testit $myarray
#----output----
# The original array is: 1 2 3 4 5
# the parameters are: 1
# the received array is 1
```

```bash
#!/bin/bash
function testit {
    local newarray
    newarray=(echo "$@")
    echo "The new array value is: ${newarray[*]}"
}
myarray=(1 2 3 4 5)
echo "The original array is ${myarray[*]}"
testit ${myarray[*]}
```

```bash
#!/bin/bash
function addarray {
	local sum=0
	local newarray
	newarray=(echo "$@")
	for value in ${newarray[*]}; do
		sum=$[ $sum+$value ]
	done
	echo $sum
}
myarray=(1 2 3 4 5)
echo "The original array is: ${myarray[*]}"
arg1=$(echo ${myarray[*]})
result=$(addarray $arg1)
echo "The result is $result"
```

###### 从函数返回数组

```bash
#!/bin/bash
function arraydblr {
	local origarry
	local newarray
	local elements
	local i
	origarry=($(echo "$@"))
	newarray=($(echo "$@"))
	elements=$[ $# -1 ]
	for (( i = 0; i <= $elements; i++ )); do
		newarray[$i]=$[ ${origarry[$i]} *2 ]
	done
	echo ${newarray[*]}
}
myarray=(1 2 3 4 5)
echo "The original array is: ${myarray[*]}"
arg1=$(echo ${myarray[*]})
result=($(arraydblr $arg1))
echo "The new array is: ${result[*]}"
#----output----
# The original array is: 1 2 3 4 5
# The new array is: 2 4 6 8 10
```

###### 函数递归

```bash
#!/bin/bash
function factorial {
	if [ $1 -eq 1 ]; then
		echo 1
	else
		local temp=$[ $1 -1 ]
		local result=$(factorial $temp)
		echo $[ $result * $1 ]
	fi
}

read -p "Enter value: " value
result=$(factorial $value)
echo "The factorial of $value is: $result"
```

###### 创建库

```bash
#!/bin/bash
# myfuncs.sh
function addem {
	echo $[ $1+$2 ]
}
function multem {
	echo $ [ $1*$2 ]
}
function divem {
	if [[ $2 -ne 0 ]]; then
		echo $[ $1/$2 ]
	else
		echo -1
	fi
}
```

```bash
#!/bin/bash
. ./myfuncs

result=$(addem 10 15)
echo "The result is $result"
```

###### 在命令行上使用函数

`function divem { echo $[ $1/$2 ]; } && divem 100 5`

`function doubleit { read -p "Enter value: " value; echo $[
$value * 2 ]; } && doubleit` 

###### 在`.bashrc`文件中定义函数

```bash
$ cat .bashrc
# 直接定义函数
function addem {
    echo $[ $1+$2 ]
}
```

```bash
$ cat .bashrc
# 读取函数文件
. /home/xxx/myfuncs
```

###### 实例 [^4] p391

`ftp://ftp.gnu.org/gnu/shtool/shtool-2.0.8.tar.gz`

`tar -zxvf shtool-2.0.8.tar.gz`

`$ ./confifgure`

`$ make`

[下一篇](shell7.md)

---

[^1]: 函数名必须唯一；函数必须先创建再使用；重定义函数后会覆盖原来函数的定义，并且不会产生任何错误信息
[^2]: bash shell会把函数当作一个小型脚本，运行结束时会返回一个退出状态码。 有==3种==不同的方法来为函数生成退出状态码。
[^3]: local关键字保证了变量只局限在该函数中。如果脚本中在该函数之外有同样名字的变量， 那么shell将会保持这两个变量的值是分离的

[^4]: 使用GNU shtool shell脚本函数库