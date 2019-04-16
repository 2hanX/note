## crunch工具的综合指南 [^1]

**语法：crunch <min> <max> [character-string] [options]**

`crunch 2 3 -o /root/Desktop/0.txt`

`crunch 3 4 raj -o /root/Desktop/1.txt`

`crunch 3 4 raj -o /root/Desktop/1.txt`

`crunch 1 3 raj\ /root/Desktop/4.txt` [^2]

`cat /usr/share/crunch/charset.txt` [^3]

`crunch 4 5 -f /usr/share/crunch/charset.txt loweralpha-numeric -o /root/Desktop/5.txt`

`crunch 6 6 -t raj%%% -o /root/Desktop/6.txt` [^4]

`crunch 6 6 -t raj%%% -d 2% -o /root/Desktop/6.1.txt` [^5]

`crunch 6 6 -t raj,,, -o /root/Desktop/7.txt`

`crunch 6 6 -t raj,,, -d 1, -o /root/Desktop/7.1.txt`

`crunch 3 6 -p raj chandel hackingarticles` [^6]

`crunch 5 5 IGNITE -c 25 -o /root/Desktop/8.txt` [^7]

`crunch 5 7 raj@123 -b 3mb -o START` [^8]

`crunch 5 7 raj@123 –z gzip -o START` [^9]

[原文](https://www.hackingarticles.in/comprehensive-guide-on-crunch-tool/)

---

[^1]: 使用最强大的工具CRUNCH为用户名或密码创建自己的wordlist
[^2]: 以下命令将使用带有字符串“raj”的空格字符（\）生成wordlist。除了使用（\）之外，您还可以在字符串周围使用双引号作为“raj”以及双引号内的空格。
[^3]: **使用crunch的字符集文件创建wordlist**
[^4]: Crunch提供**-t选项**，根据您的要求使用特定模式生成单词列表。使用选项**-t**，您可以生成以下指定的4种类型模式：使用**@**表示小写字母使用、**，**用于大写字母、使用**％**表示数字字符、使用**^**表示特殊字符符号
[^5]: **生成具有重复字符限制的wordlist**。通过**使用-d**参数和给定的模式，Crunch可以限制字符的重复。
[^6]: **使用Permutation生成单词表**。**-p**选项用于在排列的帮助下生成wordlist，这里可以忽略字符串的最小和最大长度。此外，它可以与一个单词串或多个单词串一起使用
[^7]: **生成具有有限单词的词典**如果您将观察上面的所有输出结果，那么您将发现crunch已生成字典并显示每个字典的行数。例如，文本文件0.txt具有18252行数，每行仅包含一个单词。因此，如果您希望设置过滤器，则应生成一定数量的行，然后执行下面给出的行。它将生成一个仅包含25个单词的字典，并将输出保存在8.txt中。
[^8]: 将**-b选项**用于将单个词列表拆分为多个词列表的词列表碎片。这是一个非常有用的选项，可以将GB中的单词列表划分为MB。
[^9]: Crunch让你生成带有**选项-z的**压缩wordlist，其他参数是gzip，bzip2，lzma和7z