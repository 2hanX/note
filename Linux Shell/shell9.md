##### 正则表达式 [^1]

###### 纯文本

`echo "This is a test" | sed -n '/test/p'`

`echo "This is a test" | sed -n '/trial/p'` ==N==

`echo "This is a test" | gawk '/test/{print $0}'`

###### 特殊字符 [^2]

`sed -n '/\$/p' data2`

`echo "\ is a special character" | sed -n '/\\/p'`

###### 锚字符

`echo "The book store" | sed -n '/^book/p' ` ==N==

`echo "Books are great" | sed -n '/^Book/p'`

`echo "This ^ is a test" | sed -n '/s ^/p'`

`echo "This is a good book" | sed -n '/book$/p'`

`echo "There are a lot of good books" | sed -n '/book$/p'` ==N==

`sed -n '/^this is a test$/p' data4`

`sed '/^$/d' data5` [^3]

###### 点号字符 [^4]

`sed -n '.at/p' data.txt`

###### 字符组

`sed -n '/[ch]at/p' data6`

`echo "Yes" | sed -n '/[Yy]es/p'`

`echo "Yes" | sed -n '/[Yy][Ee][Ss]/p'`

`echo "yE" | sed -n '/[Yy][Ee][Ss]/p'` ==N==

`echo "yEssdf" | sed -n '/[Yy][Ee][Ss]/p'`

`echo "yEssdf" | sed -n '/^[Yy][Ee][Ss]$/p'` ==N==

###### 排除型字符组

`sed -n '/[^ch]at/p' data6`

###### 区间

`sed -n '/^[0-9][0-9][0-9][0-9][0-9]$/p' data8`

`echo "a8392" | sed -n '/^[0-9][0-9][0-9][0-9][0-9]$/p'` ==N==

`sed -n '/[a-ch-m]at/p' data6` [^5]

###### 特殊的字符组

| 组            | 描述                                          |
| ------------- | --------------------------------------------- |
| `[[:alpha:]]` | 匹配任意字母字符，不管是大写还是小写          |
| `[[:alnum:]]` | 匹配任意字母数字字符`0~9`、`A~Z`或`a~z `      |
| `[[:blank:]]` | 匹配空格或制表符                              |
| `[[:digit:]]` | 匹配`0~9`之间的数字                           |
| `[[:lower:]]` | 匹配小写字母字符`a~z`                         |
| `[[:print:]]` | 匹配任意可打印字符                            |
| `[[:punct:]]` | 匹配标点符号                                  |
| `[[:space:]]` | 匹配任意空白字符:空格、制表符、NL、FF、VT和CR |
| `[[:upper:]]` | 匹配任意大写字母字符`A~Z`                     |

`echo "abc" | sed -n '/[[:digit:]]/p'` ==N==

`echo "abc" | sed -n '/[[:alpha:]]/p'`

`echo "abc123" | sed -n '/[[:digit:]]/p'`

`echo "This is, a test" | sed -n '/[[:punct:]]/p'`

###### 星号 [^6]

`echo "ik" | sed -n '/ie*k/p'`

`echo "iek" | sed -n '/ie*k/p'`

`echo "ieeek" | sed -n '/ie*k/p'`

`echo "this is a regular pattern expression" | sed -n /regular.*expression/p' ` [^7]

`echo "bt" | sed -n '/b[ae]*t/p'`

`echo "btt" | sed -n '/b[ae]*t/p'`

`echo "btt" | sed -n '/b[ae]*t/p'`

`echo "baakeeet" | sed -n '/b[ae]*t/p'` ==N==

##### 扩展正则表达式（ERE） [^8]

###### 问号 [^9]

`echo "bt" | gawk '/be?t/{print $0}'`

`echo "bt" | sed '/be?t/p'`

`echo "beet" | gawk '/be?t/{print $0}'` ==N==

`echo "beet" | sed '/be?t/p'`

`echo "bat" | gawk '/b[ae]?t/{print $0}'`

`echo "bot" | gawk '/b[ae]?t/{print $0}'` ==N==

`echo "baet" | gawk '/b[ae]?t/{print $0}' ` ==N==

###### 加号 [^10]

`echo "beeet" | gawk '/be+t/{print $0}'`

`echo "bt" | gawk '/be+t/{print $0}'` ==N==

###### 使用花括号 [^11] P453

`echo "bt" | gawk --re-interval '/be{1}t/{print $0}'` ==N==

`echo "bet" | gawk --re-interval '/be{1}t/{print $0}'`

`echo "beet" | gawk --re-interval '/be{1}t/{print $0}'` ==N==

`echo "bt" | gawk --re-interval '/be{1,2}t/{print $0}'` ==N==

`echo "bet" | gawk --re-interval '/be{1,2}t/{print $0}'`

`echo "beeet" | gawk --re-interval '/be{1,2}t/{print $0}'`==N==

###### 管道符号 [^12]

` echo "The cat is asleep" | gawk '/cat|dog/{print $0}'` 

`echo "The dog is asleep" | gawk '/cat|dog/{print $0}'`

`echo "He has a hat." | gawk '/[ch]at|dog/{print $0}'`

###### 表达式分组 [^13]

`echo "Sat" | gawk '/Sat(urday)?/{print $0}'`

`echo "Saturday" | gawk '/Sat(urday)?/{print $0}'`

##### 正则表达式实战

###### 目录文件计数

```bash
#!/bin/bash
mypath=$(echo $PATH | sed 's/:/ /g')
count=0
for directory in $mypath; do
	check=$(ls $directory)
	for item in $check; do
		count=$[ $count+1 ]
	done
	echo "$directory - $count"
	count=0
done
```

###### 验证电话号码

```bash
#!/bin/bash
gawk --re-interval '/^\(?[2-9][0-9]{2}\)?(| |-|\.|)[0-9]{3}( |-|.\|)[0-9]{4}$/{print $0}'
#----example-----
# (123)456-7890
# (123) 456-7890
# 123-456-7890
# 123.456.7890
```

###### 解析邮件地址

```bash
#!/bin/bash
gawk --re-interval '/^([a-zA-Z0-9_\-\.\+]+)@([a-zA-Z0-9_\-\.]+)\.([a-zA-Z]{2,5})$/{print $0}'
```

[下一篇](shell10.md)

---

[^1]: POSIX==基础正则表达式==(basic regular expression，==BRE==)引擎; POSIX==扩展正则表达式==(extended regular expression，==ERE==)引擎
[^2]: `    .*[]^${}\+?|() `
[^3]: 过滤出数据流中的空白行
[^4]: 特殊字符点号用来匹配除换行符之外的==任意单个字符==。它必须匹配一个字符，如果在点号字符的位置没有字符，那么模式就不成立
[^5]: 该字符组允许区间`a~c`、`h~m`中的字母出现在`at`文本前，但不允许出现`d~g`的字母
[^6]: 在==字符后面放置星号==表明该字符必须在匹配模式的文本中出现==0次或多次==。
[^7]: 另一个方便的特性是将点号特殊字符和星号特殊字符(`.*`)组合起来。这个组合能够匹配==任意数量的任意字符==。它通常用在数据流中两个可能相邻或不相邻的文本字符串之间。
[^8]: `ERE`模式包括了一些可供Linux应用和工具使用的额外符号。`gawk`程序能够识别`ERE`模式，但`sed`编辑器不能。
[^9]: 问号表明前面的字符可以==出现0次或1次==，但只限于 此。它不会匹配多次出现的字符。
[^10]: 加号表明前面的字符可以==出现1次或多次，但必须至少出现1次==。如果该字符没有出现，那么模式就不会匹配。
[^11]: `ERE`中的花括号允许你==为可重复的正则表达式指定一个上限==。这通常称为间隔(interval)。可以用两种格式来指定区间。`m`:正则表达式准确出现m次。   `m, n`:正则表达式至少出现m次，至多n次。
[^12]: 管道符号允许你在检查数据流时，用逻辑`OR`方式指定正则表达式引擎要用的两个或多个模式。如果任何一个模式匹配了数据流文本，文本就通过测试。如果没有模式匹配，则数据流文本匹配失败. `expr1|expr2|...`
[^13]: 正则表达式模式也可以用==圆括号==进行分组。当你将正则表达式模式分组时，该组会被视为==一 个标准字符==。可以像对普通字符一样给该组使用特殊字符。