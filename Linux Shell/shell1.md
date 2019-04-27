###### 数组变量

`mytest=(one two three four five)`

`echo $mytest` [^1]

`echo ${mytest[2]}` [^2]

`echo ${mytest[*]}`

`mytest[1]=six`

`unset mytest[1]` [^3]

`unset mytest`

###### 修改 `shell,finger`

`chsh -s /bin/csh test`

`chfn test`

`finger test`

###### 编辑组

`groupadd shared`

`usermod -G shared rich`

`usermod -G shared test` [^4]

###### 文件属性

`umask` [^5]

###### 改权限

`    [ugoa...][[+-=][rwxXstugo...]`

`chmod o+x test`

`chmod u-r test`

`ls -F test` [^6]

###### 共享文件 [^7]

- 设置用户ID(SUID) [^8]
- 设置组ID(SGID) [^9]
- 粘着位 [^10]

`groupadd shared`

`mkdir testdir`

`chgrp shared testdir`

`chmod g+s testdir`

`umask`

`cd testdir && touch testfile`

###### 从源码安装 [^11]

`tar -zxvf demo.tar.gz`

`cd demo && ls`

`./configure && make && make install` 

###### 编辑脚本

`echo -n "The time and date are: "` [^12]

`echo "The cost of the item is \$15"` [^13]

`value1=10 ;value2=$value1;echo The result value is $value2` [^14]

###### 命令替换 [^15]

- 反引号字符(`)

  ```bash
  testing=`date`
  echo -n "The date and time are: "
  $testing
  ```

- ` $()`格式

  ```bash
  testing=$(date)
  echo -n "The date and time are: "
  $testing
  ```

  ```
  today=$(date +%y%m%d)
  ls /usr/bin -al > log.$today
  ```

###### 输出重定向

`date > test6`

`date >> test6` [^16]

###### 输入重定向

`wc < test6.txt` [^17]

###### 内联输入重定向 [^18]

```bash
$ wc << EOF
> test string 1
> test string 2
> EOF
2	6	29
```

###### 执行数学运算

`expr 1 + 5`

`expr 2 \* 5`      ~~`expr 2 * 5`~~

`echo $[2*5]`

###### 浮点解决方案

- z shell (zsh)

- `bc` 内建的 bash 计算器 [^19]

```bash
$ bc -q
var1=3
var1/6
0
scale=4
var1/6
.5000
quit
```

```bash
#!/bin/bash
#variable=$(echo "options; expression" | bc)
var1=$(echo "scale=4; 3.44 / 5" | bc)
echo The answer is $var1
//$var1=.6880
```

```bash
#!/bin/bash
var1=20
var2=3.14159
var3=$(echo "scale=4; $var1 * $var1" | bc)
var4=$(echo "scale=4; $var3 * $var2" | bc)
echo The final result is $var4
```

```bash
#!/bin/bash
#variable=$(bc << EOF
#options
#statements
#expressions
#EOF 
#)
var1=10.46
var2=43.67
var3=33.2
var4=71
var5=$(bc << EOF
scale = 4
a1 = ( $var1 * $var2)
b1 = ($var3 * $var4)
a1 + b1
EOF
)
echo The final answer for this mess is $var5
```

###### 退出脚本

`echo $?` [^20]

| 状态码 | 描述                       |
| ------ | -------------------------- |
| 0      | 命令成功结束               |
| 1      | 一般性未知错误             |
| 2      | 不适合的shell命令          |
| 126    | 命令不可执行               |
| 127    | 没找到命令                 |
| 128    | 无效的退出参数             |
| 128+x  | 与Linux信号x相关的严重错误 |
| 130    | 通过Ctrl+C终止的命令       |
| 255    | 正常范围之外的退出状态码   |

`exit 5` [^21]

###### 结构化命令 [^22]

```bash
#!/bin/bash
# testing the if statement
#if pwd ; then
#  echo "It worked"
#fi
if pwd
then
   echo "It worked"
fi
```

```bash
#!/bin/bash
# testing the else section
#
testuser=NoSuchUser
#
if grep $testuser /etc/passwd 
then
   echo "The bash files for user $testuser are:"
   ls -a /home/$testuser/.b*
   echo
else
   echo "The user $testuser does not exist on this system."
   echo
fi
```

###### 条件测试方法

- test命令

```bash
#!/bin/bash
# Testing the test command #
# if test condition ; then
# commands
# fi
if test
then
   echo "No expression returns a True"
else
   echo "No expression returns a False"
fi
$
$ ./test6.sh
No expression returns a False 
```

- [condition] [^23]

```bash
if [ condition ] then
commands
fi
```

```bash
#!/bin/bash
file=$PWD/test2ss
if [ $file ]; then
	echo "file $file exist."
fi
#-------------
# if test -e $file ; then
# 	echo "file $file exist."
# fi
```

###### 数值比较

| 比较        | 描述                   |
| ----------- | ---------------------- |
| `n1 -eq n2` | 检查n1是否与n2相等     |
| `n1 -ge n2` | 检查n1是否大于或等于n2 |
| `n1 -gt n2` | 检查n1是否大于n2       |
| `n1 -le n2` | 检查n1是否小于或等于n2 |
| `n1 -lt n2` | 检查n1是否小于n2       |
| `n1 -ne n2` | 检查n1是否不等于n2     |

```bash
#!/bin/zsh
var1=6
var2=3
if [ $var1 -gt 5 ];then
	echo "the test $var1 is greater than 5."
fi
if [ $var1 -eq $var2 ] ; then
	echo "the $var1 equal $var2"
else
	echo "the $var1 not equal $var2"
fi

```

###### 字符串比较

| 比较                | 描述                           |
| ------------------- | ------------------------------ |
| `str1 = str2`       | 检查str1的==值==是否和str2相同 |
| `str1 != str2 `     | 检查str1的==值==是否和str2不同 |
| `-z str1`           | 检查str1的==长度==是否为0      |
| `str1 < str2` [^24] | 检查str1是否比str2小           |
| `str1 > str2`       | 检查str1是否比str2大           |
| `-n str1`           | 检查str1的==长度==是否非0      |

```bash
#!/bin/bash
string1="zxcx"
string2="axcx"
if [ $string1 \> $string2 ];then
	echo "$string1 > $string2"
else
	echo "$string1 ≯ $string2"
fi
```

###### 文件比较

| 比较              | 描述                                     |
| ----------------- | ---------------------------------------- |
| `-d file`         | 检查file是否存在并是一个目录             |
| `-e file`         | 检查file是否存在                         |
| `-f file`         | 检查file是否存在并是一个文件             |
| `-r file`         | 检查file是否存在并可读                   |
| `-s file`         | 检查file是否存在并非空                   |
| `-w file`         | 检查file是否存在并可写                   |
| `-x file`         | 检查file是否存在并可执行                 |
| `-O file`         | 检查file是否存在并属当前用户所有         |
| `-G file`         | 检查file是否存在并且默认组与当前用户相同 |
| `file1 -nt file2` | 检查file1是否比file2新                   |
| `file1 -ot file2` | 检查file1是否比file2旧                   |

```bash
#!/bin/bash
jump_directory=$PWD/test
if [ -d $jump_directory ]; then
	echo "The $jump_directory directory exist."
	cd $jump_directory && ls
else
	echo "The $jump_directory directory not exist."
fi
```

```bash
#!/bin/zsh
file=$PWD/demo.sh
if [ -O $file ];then
	echo "You are the owner of the file"
else
	echo "Sorry, you are not the owner of the file"
fi
```

###### 复合条件测试

- `[ condition1 ] && [ condition2 ]`
- `[ condition1 ] || [ condition2 ]`

```bash
#!/bin/zsh
file1=$PWD/test2
file2=$PWD/test3
if [ -e $file1 ] && [ -w $file2 ];then
	echo "The file exists and you can write to it"
else
	echo "I cannot write to the file"
fi
```

###### **`if-then`**的高级特性

- 用于数学表达式的双括号 [^25]

  

  - | 符号    | 描述     |
    | ------- | -------- |
    | `val++` | 后增     |
    | `val--` | 后减     |
    | `++val` | 先增     |
    | `--val` | 先减     |
    | `!`     | 逻辑求反 |
    | `~`     | 位求反   |
    | `**`    | 幂运算   |
    | `<<`    | 左位移   |
    | `>>`    | 右位移   |
    | `&`     | 位布尔和 |
    | `|`     | 位布尔或 |
    | `&&`    | 逻辑和   |
    | `||`    | 逻辑或   |

    ```bash
    #!/bin/zsh
    val1=10
    if (( $val1 ** 2 > 90 )); then 
    	((val2 = $val1 ** 2 ))
    	echo "The square of $val1 is $val2"
    fi
    # 注意，不需要将双括号中表达式里的大于号转义。这是双括号
    # 命令提供的另一个高级特性
    ```

- 用于高级字符串处理功能的双方括号 [^26]

  ​	

  ```bash
  #!/bin/zsh
  if [[ $USER == e* ]]; then
  	echo "hello $USER"
  else
  	echo "sorry, I do not know you"
  fi
  #右边的字符串(e*)视为一个模式,并应用模式匹配规则(正则表达式)
  ```

###### **`case`**命令 [^27]

```bash
#!/bin/zsh
case $USER in
	eric)
		echo "Welcome, $USER"
		echo "enjoy yourself"
		;;
	testing|rich )
		echo "Special testing account"
		;;
	*)
		echo "Sorry, you are not allowed here"
		;;
esac
```

[下一篇](shell2.md)

---



[^1]: 只有数组的第一个值显示出来
[^2]: 引用一个单独的数组元素，就必须用代表它在数组中位 置的数值索引值;索引从0开始
[^3]: `unset`命令删除在索引值为1的位置上的值。显示整个数组时，看起来像是索引 里面已经没这个索引了。但当专门显示索引值为1的位置上的值时，就能看到这个位置是空的
[^4]: 如果更改了已登录系统账户所属的用户组，该用户必须登出系统后再登录，组关系的更 改才能生效;为用户账户分配组时要格外小心。如果加了`-g`选项，指定的组名会替换掉该账户的默认 组。`-G`选项则将该组添加到用户的属组的列表里，不会影响默认组
[^5]: 对文件来说，全权限的值是666;而对目录来说，则是777;`umask`值从对象的全权限值中减掉
[^6]: ls命令的`-F`选项，它能够在具有执行权限的文件名后 加一个星号
[^7]: Linux系统上共享文件的方法是创建组
[^8]: 当文件被用户使用时，程序会以文件属主的权限运行
[^9]: 对文件来说，程序会以文件属组的权限运行;对目录来说，目录中创建的新文件会以目录的默认属组作为默认属组
[^10]: 进程结束后文件还驻留(粘着)在内存中
[^11]: 如何从`tarball`来解包和安装软件
[^12]: `-n` 把文本字符串和命令输出显示在同一行中
[^13]: 脚本在引号中出现美元符，它就会以为你在引用一个变量。要显示美元符，你必须在它前面放置一个反斜线
[^14]: 重要的是要记住，引用一个变量值时需要使用美元符，而引用变量来对其进行赋值时则不要使用美元符
[^15]: 从命令输出中提取信息，并将其赋给变量
[^16]: `>>`不覆盖文件原有内容，而是将命令的输出追加到已有文件中
[^17]: `wc`命令可以对数据中的文本进行计数。默认情况下，它会输出3个值:  文本的行数、文本的词数、文本的字节数
[^18]: 这种方法无需使用文件进行重定向，只需要在命令行中指定用于输入重定向的数据;此外必须指定一个==文本标记==来划分输入数据的开始和结尾。==任何字符串都可作为文本标记==，但在数据的开始和结尾文本标记必须一致；shell会用`PS2`环境变量中定义的次提示符来提示输入数据
[^19]: `-q`命令行选项可以不显示bash计算器冗长的欢迎信息;`scale`设置在计算结果中保留的小数位数;`quit`退出bash计算器
[^20]: 查看上个已执行命令的退出状态码;一个成功结束的命令的退出状态码是0。如果一个命令结束时有错误，退出状态码就是一个正数值
[^21]: `exit`命令允许你在脚本结束时指定一个退出状态码;退出状态码最大只能是`255`;退出状态码如果超过255，就按照==指定的数值除以256后得到的余数==
[^22]: bash shell的`if`语句会运行`if`后面的那个命令。前提是该命令的退出状态码是`0 `(该命令成功运行)，位于`then`部分的命令就会被执行
[^23]: 方括号定义了测试条件。注意，第一个方括号之后和第二个方括号之前必须加上一个==空格==， 否则就会报错 
[^24]: ==大于号和小于号必须转义==，否则shell会把它们当作重定向符号，把字符串值当作文件名;大于和小于顺序和`sort`命令所采用的不同;==比较测试==中使用的是标准的ASCII顺序(==小写字符>大写字符)==，根据每个字符的ASCII数值来决定排序结果。`sort` 命令使用的是系统的本地化语言设置中定义的排序顺序(==小写字符<大写字符==)
[^25]: ==双括号命令==允许你在比较过程中使用高级数学表达式。`test`命令只能在比较中使用简单的算术操作。双括号命令提供了更多的数学符号
[^26]: 使用了`test`命令中采用的标准字符串比较;提供的另一个特性——==模式匹配==(pattern matching)
[^27]: `case`命令会采用列表格式来检查单个变量的多个值 

