###### 内建函数 [^1]

| 数学函数      | 描述                          |
| ------------- | ----------------------------- |
| `atan2(x, y)` | x/y的反正切，x和y以弧度为单位 |
| `cos(x)`      | x的余弦，x以弧度为单位        |
| `exp(x)`      | x的指数函数(e的 x 次幂)       |
| `int(x)`      | x的整数部分，取靠近零一侧的值 |
| `log(x)`      | x的自然对数                   |
| `rand( )`     | 比0大比1小的随机浮点值        |
| `sin(x)`      | x的正弦，x以弧度为单位        |
| `sqrt(x)`     | x的平方根                     |
| `srand(x)`    | 为计算随机数指定一个种子值    |

`gawk 'BEGIN{x=int(10.43);print x}'`

| 按位操作数据的函数   | 描述                       |
| -------------------- | -------------------------- |
| `and(v1, v2)`        | 执行值v1和v2的按位与运算   |
| `compl(val)`         | 执行val的补运算            |
| `lshift(val, count)` | 将值val左移count位         |
| `or(v1, v2)`         | 执行值v1和v2的按位或运算   |
| `rshift(val, count)` | 将值val右移count位         |
| `xor(v1, v2)`        | 执行值v1和v2的按位异或运算 |

`gawk 'BEGIN{print xor(2,4)}'`

`gawk 'BEGIN{print rshift(20,3)}'`

| 字符串函数                   | 描述                                                         |
| :--------------------------- | ------------------------------------------------------------ |
| `asort(s [,d]) `             | 将数组s按数据元素值排序。索引值会被替换成表示新的排序顺序的连续数字。另外， 如果指定了d，则排序后的数组会存储在数组d中 |
| `asorti(s [,d])`             | 将数组s按索引值排序。生成的数组会将索引值作为数据元素值，用连续数字索引来表 明排序顺序。另外如果指定了d，排序后的数组会存储在数组d中 |
| `gensub(r, s, h [, t]) `     | 查找变量$0或目标字符串t(如果提供了的话)来匹配正则表达式r。如果h是一个以g 或G开头的字符串，就用s替换掉匹配的文本。如果h是一个数字，它表示要替换掉第h 处r匹配的地方 |
| `gsub(r, s [,t]) `           | 查找变量$0或目标字符串t(如果提供了的话)来匹配正则表达式r。如果找到了，就 全部替换成字符串s |
| `index(s, t)`                | 返回字符串t在字符串s中的索引值，如果没找到的话返回0          |
| `length([s])`                | 返回字符串s的长度;如果没有指定的话，返回`$0`的长度           |
| `match(s, r [,a])`           | 返回字符串s中正则表达式r出现位置的索引。如果指定了数组a，它会存储s中匹配正 则表达式的那部分 |
| `split(s, a [,r])`           | 将s用FS字符或正则表达式r(如果指定了的话)分开放到数组a中。返回字段的总数 |
| `sprintf(format, variables)` | 用提供的format和variables返回一个类似于printf输出的字符串    |
| `sub(r, s [,t])`             | 在变量$0或目标字符串t中查找正则表达式r的匹配。如果找到了，就用字符串s替换 掉第一处匹配 |
| `substr(s, i [,n])`          | 返回s中从索引值i开始的n个字符组成的子字符串。如果未提供n，则返回s剩下的部 分 |
| `tolower(s)`                 | 将s中的所有字符转换成小写                                    |
| `toupper(s)`                 | 将s中的所有字符转换成大写                                    |

`gawk 'BEGIN{x="new string";print toupper(x);print length(x)}'`

```bash
gawk 'BEGIN{
 var["a"] = 1
 var["g"] = 2
 var["m"] = 3 
 var["u"] = 4
 asort(var, test)
 for (i in test)
	 print "Index:",i," - value:",test[i]
}'
#---output---
# Index: 1  - value: 1
# Index: 2  - value: 2
# Index: 3  - value: 3
# Index: 4  - value: 4
```

| 时间函数                        | 描述                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| `mktime(datespec)  `            | 将一个按YYYY MM DD HH MM SS [DST]格式指定的日期转换成时间戳值 |
| `strftime(format [,timestamp])` | 将当前时间的时间戳或timestamp(如果提供了的话)转化格式化日期(采用shell函数date()的格式) |
| `systime( )`                    | 返回当前时间的时间戳                                         |

```bash
gawk 'BEGIN{
	date=systime()
	print date
	print strftime("%A,%B %d,%Y",date)
}'
```

###### 自定义函数 [^2]

```bash
gawk '
	function myrand(limit){
		return int(limit*rand())
	}
	BEGIN{
	print myrand(100)
}'
```

```bash
gawk '
	 function myprint(){
		 printf "%-16s - %s\n", $1, $4
	 }
	BEGIN{FS="\n"; RS=""}
	{
		myprint()
}' data2
```

###### 创建函数库

`gawk -f funclib -f script4 data2`

###### 实例

```bash
# cat data2
Rich Blum,team1,100,115,95
Barbara Blum,team1,110,115,100
Christine Bresnahan,team2,120,115,118
Tim Bresnahan,team2,125,112,116
```

```bash
gawk '
function sum(){
	return $3+$4+$5
}
function avg(){
	return sum()/3.
}
BEGIN{
	FS=","
}{
	printf "Name: %-25s - Sum: %-4d - Avg:
	%-3.1f\n",$1,sum(),avg()
}' data2
#---output---
Name: Rich Blum                 - Sum: 310  - Avg: 103.3
Name: Barbara Blum              - Sum: 325  - Avg: 108.3
Name: Christine Bresnahan       - Sum: 353  - Avg: 117.7
Name: Tim Bresnahan             - Sum: 353  - Avg: 117.7
```

```bash
#!/bin/bash
for team in $(gawk -F, '{print $2}' data2 | uniq); do
    gawk -v team=$team '
    BEGIN{
        FS=",";total=0
    }
    {
        if ($2 == team)
            total += $3+$4+$5
    }
    END{
        avg=total/6;
        print "Total for", team,"is",total,",the average is",avg
    }' data2
done
#---output---
Total for team1 is 635 ,the average is 105.833
Total for team2 is 706 ,the average is 117.667
```



---

[^1]: gawk编程语言提供了不少内置函数，可进行一些常见的数学、字符串以及时间函数运算
[^2]: 要定义自己的函数，必须用`function`关键字` function name([variables]) {statements }`