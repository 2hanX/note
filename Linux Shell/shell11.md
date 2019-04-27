###### 创建`sed`实用工具

###### 加倍行间距

`sed 'G' file.txt`

`sed '$!G' file.txt`

###### 给文件中的行编号

`sed '=' lsof.txt | sed 'N; s/\n/ /'`

`nl lsof.txt`

`cat -n lsof.txt`

###### 打印末尾行

`sed -n '$p' lsof.txt`

```shell
$ sed '{
> :start
> $q ; N ; 11,$D > b start
> }' data7.txt

# 滚动窗口是检验模式空间中文本行块的常用方法，它使用N命令将这些块合并起来。N命令将下一行文本附加到模式空间中已有文本行后面。一旦你在模式空间有了一个10行的文本块，你可以用美元符来检查你是否已经处于数据流的尾部。如果不在，就继续向模式空间增加行，同时删除原来的行(记住，D命令会删除模式空间的第一行)。
```

###### 删除连续的空白行

`sed '/./,/^$/!d' file.txt `

###### 删除开头的空白行

`sed '/./,$!d' data9.txt`

###### 删除结尾的空白行

```bash
$ sed '{
> :start
> /^\n*$/{$d ; N ; b start } 
> }' data10.txt
```

###### 删除HTML标签

`sed 's/<[^>]*>//g; /^$/d' file.txt`

# `gawk`进阶

###### 内建变量

| 变量          | 描述                                             |
| ------------- | ------------------------------------------------ |
| `FIELDWIDTHS` | 由空格分隔的一列数字，定义了每个数据字段确切宽度 |
| `FS`          | 输入字段分隔符                                   |
| `RS`          | 输入记录分隔符                                   |
| `OFS`         | 输出字段分隔符                                   |
| `ORS`         | 输出记录分隔符                                   |

`gawk 'BEGIN FS=',';{print $1,$2,$3}' file.txt `

`gawk 'BEGIN{FS=",";OFS="--"} {print $1,$2,$3,$4}' file.txt`

`gawk 'BEGIN{FIELDWIDTHS="3 5 2 5"}{print $1,$2,$3,$4}'file.txt` [^1]

`    gawk 'BEGIN{FS="\n";RS=""}{print $1,$4}'file txt` [^2]

###### 数据变量

| 变量         | 描述                                                 |
| ------------ | ---------------------------------------------------- |
| `ARGC`       | 当前命令行参数个数                                   |
| `ARGIND`     | 当前文件在ARGV中的位置                               |
| `ARGV`       | 包含命令行参数的数组                                 |
| `CONVFMT`    | 数字的转换格式(参见printf语句)，默认值为%.6 g        |
| `ENVIRON`    | 当前shell环境变量及其值组成的关联数组                |
| `ERRNO`      | 当读取或关闭输入文件发生错误时的系统错误号           |
| `FILENAME`   | 用作gawk输入数据的数据文件的文件名                   |
| `FNR`        | 当前数据文件中的数据行数                             |
| `IGNORECASE` | 设成非零值时，忽略gawk命令中出现的字符串的字符大小写 |
| `NF`         | 数据文件中的字段总数                                 |
| `NR`         | 已处理的输入记录数                                   |
| `OFMT`       | 数字的输出格式，默认值为%.6 g                        |
| `RLENGTH`    | 由match函数所匹配的子字符串的长度                    |
| `RSTART`     | 由match函数所匹配的子字符串的起始位置                |

`gawk 'BEGIN{print ARGC,ARGV[1]}' file.txt` [^3]

`gawk 'BEGIN{print ENVIRON["HOME"];print ENVIRON["PATH"]}'` 

`gawk 'BEGIN{FS=":";OFS="--"}{print $1,$NF}' /etc/passwd  ` 

`gawk 'BEGIN{FS=","}{print $1,"FNR="FNR}' file.txt`

`gawk 'BEGIN{FS=" "}{print $1,"FNR="FNR,"NR="NR}END{print "There were " NR " records processed"}'`

###### 自定义变量 [^4]

`gawk 'BEGIN{test="this is a test";print test}'`

`gawk 'BEGIN{test="this is a test";print test;test=45;print test}'`

`gawk 'BEGIN{ans=4;ans=4*ans-3;print ans}'`

###### 在命令行上给变量赋值

```bash
#!/bin/bash
BEGIN{FS=" "}
{print $n}
#---input---
# $ gawk -f script1 n=2 file.txt
# $ gawk -v n=2 -f script1 file.txt
```

###### 处理数组 [^5]

`gawk 'BEGIN{capital["who"]="eric";capital["age"]="23";print capital["age"]}'`

###### 遍历数组变量

```bash
gawk 'BEGIN{
    var[1]="who";
    var[2]="are";
    var[3]="you?";
    for (test in var){
        print "Msg:",test,"-",var[test]
    }
}'
#---output---
# Msg: 1 - who
# Msg: 2 - are
# Msg: 3 - you?
```

###### 删除数组变量

`delete var[3]`

###### 使用模式 [^6]

`gawk 'BEGIN{FS=" "} /13/{print $1}' file.txt`

###### 匹配操作符 [^7]

`$1 ~ /^data/` [^8]

`gawk 'BEGIN{FS=" "} $2 ~ /^data2/{print $1}'file.txt`

`gawk -F: '$1 ~ /root/{print $1,$NF}' /etc/passwd`

`gawk -F: '$1 !~ /root/{print $1,$NF}' /etc/passwd`

###### 数学表达式

`gawk -F: '$4  == 0 {print $1}' /etc/passwd`

`gawk -F\ '$2  == "data24" {print "find:" $2}' file.txt`

###### 结构化命令

`gawk '{if (FNR ==4) print "yes"}' file.txt`

`gawk '{if (NF ==2 ) print "NF="NF; else print "NF=no"}' file.txt`

```bash
gawk '{
    i = 1
    total = 0
    while (i<= NF){
        total += $i
        i++
    }
    avg=total/NF
    print "average: ",avg
}'  lsof.txt
```

```bash
gawk '{
    i = 1
    total = 0
    do{
        total += $i
        i++
    }while(total < 150)
    print total
}' lsof.txt
```

```bash
 gawk '{
 	total = 0
 	for (i = 1; i < 4; i++) {
		total += $i
	}
	avg = total / 3
	print "Average:",avg
}' lsof.txt
```

###### 格式化打印 [^9]

| 控制字母 | 描述                                      |
| -------- | ----------------------------------------- |
| `c`      | 将一个数作为ASCII字符显示                 |
| `d`      | 显示一个整数值                            |
| `i`      | 显示一个整数值(跟d一样)                   |
| `e`      | 用科学计数法显示一个数                    |
| `f`      | 显示一个浮点值                            |
| `g`      | 用科学计数法或浮点数显示 (选择较短的格式) |
| `o`      | 显示一个八进制值                          |
| `s`      | 显示一个文本字符串                        |
| `x`      | 显示一个十六进制值                        |
| `X`      | 显示一个十六进制值，但用大写字母A~F       |

```bash
gawk 'BEGIN{
	x=10*100
	printf "The answer is: %e\n",x
}'
```

`gawk 'BEGIN{FS="\n";RS=""}{print $1,$2}' data2`

`gawk 'BEGIN{FS="\n";RS=""}{printf "%s %s\n",$1,$4}' data2`

`gawk 'BEGIN{FS=" "}{printf "%s ",$1} END{printf "\n"}' data1`

`gawk 'BEGIN{FS="\n"; RS=""} {printf "%16s  %s\n", $1, $4}' data2`

`gawk 'BEGIN{FS="\n"; RS=""} {printf "%-16s  %s\n", $1, $4}' data2`

[下一篇](shell12.md)

---

[^1]: `FIELDWIDTHS`变量允许你不依靠字段分隔符来读取记录,一旦设置了`FIELDWIDTH`变量，gawk就会忽略`FS`变量，并根据提供的字段宽度来计算字段 输入：`1005.3247596.37 ` 输出：`100 5.324 75 96.37`
[^2]: 变量`RS`和`ORS`定义了gawk程序如何处理数据流中的字段。默认情况下，gawk将`RS`和`ORS`设为 ==换行符==。默认的`RS`值表明，输入数据流中的每行新文本就是一条新纪录。
[^3]: `ARGC`和`ARGV`变量允许从shell中获得命令行参数的总数以及它们的值。但这可能有点麻烦，因为gawk并不会将程序脚本当成命令行参数的一部分。
[^4]: gawk自定义变量名可以是任意数目的字母、数字和下划线，但不能以数字开头。重要的是，要记住gawk变量名区分大小写。
[^5]: 可以用标准赋值语句来定义数组变量。数组变量赋值的格式如下:  					`var[index] = element`
[^6]: `BEGIN`和`END`关键字是用来在读取数据流之前或之 后执行命令的特殊模式。类似地，你可以创建其他模式在数据流中出现匹配数据时执行一些命令。
[^7]: 匹配操作符(matching operator)允许将正则表达式限定在记录中的特定数据字段。匹配操作符是波浪线(`~`)。可以指定匹配操作符、数据字段变量以及要匹配的正则表达式。
[^8]: `$1`变量代表记录中的第一个数据字段。这个表达式会过滤出第一个字段以文本`data`开头的
[^9]: 除了控制字母外，还有3种修饰符可以用来进一步控制输出。  `width`:指定了输出字段最小宽度的数字值。如果输出短于这个值，printf会将文本右对齐，并用空格进行填充。如果输出比指定的宽度还要长，则按照实际的长度输出。 `prec`:这是一个数字值，指定了浮点数中小数点后面位数，或者文本字符串中显示的最大字符数。 `-`(减号):指明在向格式化空间中放入数据时采用左对齐而不是右对齐。