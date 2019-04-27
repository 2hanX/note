# sed进阶 [^1] P460

`sed '/header/{n ; d}' file.txt ` [^2]

###### 合并文本行

`sed '/first/{ N ; s/\n/ / }' data2.txt` [^3]

`sed 'N ; s/System Administrator/Desktop User/' data3.txt`

`sed 'N ; s/System.Administrator/Desktop User/' data3.txt` [^4]

```shell
$ sed 'N
> s/System\nAdministrator/Desktop\nUser/ 
> s/System Administrator/Desktop User/
> ' data3.txt
# 但这个脚本中仍有个小问题。这个脚本总是在执行sed编辑器命令前将下一行文本读入到模式空间。当它到了最后一行文本时，就没有下一行可读了，所以N命令会叫sed编辑器停止
```

```shell
$ sed '
> s/System Administrator/Desktop User/ 
> N
> s/System\nAdministrator/Desktop\nUser/ 
> ' data4.txt
```

###### 多行删除命令

`sed 'N ; /System\nAdministrator/d' data4.txt`

`sed 'N ; /System\nAdministrator/D' data4.txt`  [^5]

`sed '/^$/{N ; /header/D}' data5.txt` [^6]

###### 多行打印命令 [^7]

`sed -n 'N ; /System\nAdministrator/P' data3.txt`

###### 保持空间 [^8] P465

| 命令 | 描述                         |
| ---- | ---------------------------- |
| h    | 将模式空间复制到保持空间     |
| H    | 将模式空间附加到保持空间     |
| g    | 将保持空间复制到模式空间     |
| G    | 将保持空间附加到模式空间     |
| x    | 交换模式空间和保持空间的内容 |

`sed -n '/first/ {h ; p ; n ; p ; g ; p }' data2.txt`

###### 排除命令 [^9]

`sed -n '/first/!p' lsof.txt`

```shell
$ sed '$!N;
> s/System\nAdministrator/Desktop\nUser/ 
> s/System Administrator/Desktop User/
> ' data4.txt
# 美元符表示数据流中 的最后一行文本，所以当sed编辑器到了最后一行时，它没有执行N命令，但它对所有其他行都执行了这个命令。
```

```shell
#反转数据流中文本行的顺序
$ sed -n '{1!G ; h ; $p }' data2.txt
```

###### 改变流 [^10]

`sed '{2,3d ; s/This is/Is this/ ; s/line./test?/}' data2.txt'`

```shell
$ sed '{/first/b jump1 ; s/This is the/No jump on/ 
> :jump1
> s/This is the/Jump here on/}' data2.txt
# 定义一个要跳转到的标签。标签以冒号开始，最多可以是7个字符长度。
```

```shell
$ echo "This, is, a, test, to, remove, commas." | sed -n '{ > :start
> s/,//1p
> /,/b start
> }'
#------output------
# This is, a, test, to, remove, commas. 
# This is a, test, to, remove, commas.
# This is a test, to, remove, commas.
# This is a test to, remove, commas. 
# This is a test to remove, commas. 
# This is a test to remove commas.
```

###### 测试 [^11]

```shell
$ sed '{
> s/first/matched/
> t
> s/This is the/No match on/ > }' data2.txt
#第一个替换命令会查找模式文本first。如果匹配了行中的模式，它就会替换文本，而且测试命令会跳过后面的替换命令。如果第一个替换命令未能匹配模式，第二个替换命令就会被执行
```

###### 模式替代

`echo "The cat sleeps in his cat." | sed 's/.at/"&"/g'` [^12]

```shell
$ echo "The System Administrator manual" | sed '
> s/\(System\) Administrator/\1 User/'
# 替代单独的单词
# sed编辑器用圆括号来定义替换模式中的子模式。你可以在替代模式中使用特殊字符来引用每个子模式。替代字符由反斜线和数字组成。数字表明子模式的位置。sed编辑器会给第一个子 模式分配字符\1，给第二个子模式分配字符\2，依此类推。
#---output---
# The System User manual
```

`echo "That furry cat is pretty" | sed 's/furry \(.at\)/\1/'` [^13]

```shell

$ echo "1234567" | sed '{
> :start
> s/\(.*[0-9]\)\([0-9]\{3\}\)/\1,\2/ 
> t start
> }'
# 第一个子模式`.*[0-9]`是以数字结尾的任意长度的字符。第二个子模式`[0-9]{3}`是若干组三位数字
```

###### 使用包装脚本 [^14]

```bash
#!/bin/bash
sed -n '1!G;h;$p' $1
```

###### 重定向`sed`的输出 [^15]

```bash
#!/bin/bash
factorial=1
counter=1
number=$1
while [[ $counter -le $number ]]; do
	factorial=$[ $factorial* $counter ]
	counter=$[ $counter+1 ]
done
result=$(echo $factorial | sed '{
	:start
	s/\(.*[0-9]\)\([0-9]\{3\}\)/\1,\2/
	t start
}')
echo "The result is $result"
# ---input---
# ./test2 20
# ---output---
# The result is 2,432,902,008,176,640,000
```

[下一篇](shell11.md)

---

[^1]: `N`:将数据流中的下一行加进来创建一个多行组(multiline group)来处理。  `D`:删除多行组中的一行。  `P`:打印多行组中的一行。
[^2]: 脚本要查找含有单词`header`的那一行。找到之后，`n`命令会让sed编辑器移动到文本的下一行，也就是那个空行,该命令列表使用`d`命令来删除空白行
[^3]: sed编辑器脚本查找含有单词`first`的那行文本。找到该行后，它会用`N`命令将下一行合并到那行，然后用替换命令`s`将换行符替换成空格。结果是，文本文件中的两行在sed编辑器的输出中成了一行。
[^4]: 通配符模式(`.`)来匹配==空格和换行符== 。但当它匹配了换行符时，它就从字符串中删掉了换行符，导致两行合并成一行。这可能不是你想要的。
[^5]: sed编辑器提供了多行删除命令`D`，它只删除模式空间中的==第一行==。该命令会删除到换行符(含换行符)为止的所有字符
[^6]: sed编辑器脚本会查找空白行，然后用`N`命令来将下一文本行添加到模式空间。如果新的模式空间内容含有单词`header`，则`D`命令会删除模式空间中的第一行。如果不结合使用`N`命令和`D`命令，就不可能在不删除其他空白行的情况下只删除第一个空白行。
[^7]: 多行打印命令(`P`)沿用了同样的 方法。它只打印多行模式空间中的第一行。这包括模式空间中直到换行符为止的所有字符
[^8]: `sed`编辑器有另一块称作保持空间(hold space)的缓冲区域。在处理模式空间中的某些行时， 可以用保持空间来==临时保存一些行==。有5条命令可用来操作保持空间
[^9]: 感叹号命令(`!`)用来排除(negate)命令，也就是让原本会起作用的命令不起作用
[^10]: sed编辑器提供了 一种方法，可以基于地址、地址模式或地址区间排除一整块命令。这允许你只对数据流中的特定 行执行一组命令。==分支==(branch)命令`b`的格式如下:` [address]b [label]`. `address`参数决定了哪些行的数据会触发分支命令。`label`参数定义了要跳转到的位置。如 果没有加label参数，跳转命令会跳转到脚本的结尾。
[^11]: 测试(test)命令(`t`)也可以用来改变sed编辑器脚本的执行流程。测试命令会根据替换命令的结果跳转到==某个标签==，而不是根据地址进行跳转。`[address]t [label]`
[^12]: `The "cat" sleep in his "hat"`
[^13]: `That cat is pretty`
[^14]: 可以将sed编辑器命令放到shell包装脚本(wrapper)中，不用每次使用时都重新键入整个脚本。包装脚本充当着sed编辑器脚本和命令行之间的中间人角色。
[^15]: 在脚本中用`$()`将sed编辑器命令的输出重定向到一个变量中，以备后用.