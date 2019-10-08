这道题是属于二进制隐藏数据的一种典型案例，在此做个记录

### 0x0 分析

文本文件中的TTL值只有4种：`63/255/127/191`，并且根据题目的介绍我们知道数据隐藏在这些值里，那么就比较容易想到的是信息隐藏在二进制中，做个比对就可以发现二进制的前两个值不同

| 值（D） | 二进制     |
| ------- | ---------- |
| 63      | `00111111` |
| 255     | `11111111` |
| 127     | `01111111` |
| 191     | `10111111` |

### 0x1 脚本提取 

```python
import binascii

tmp =""
ans =""

with open('..\\ttl.txt','r') as f:
	for line in f:
		if int(line[4:]) == 127:
			tmp += '01'
		elif int(line[4:]) == 63:
			tmp += '00'
		elif int(line[4:]) == 255:
			tmp += '11'
		elif int(line[4:]) == 191:
			tmp += '10'

for i in range(0,len(tmp),8):
	ans += chr(int(tmp[i:i+8],2))
ans = binascii.unhexlify(ans)
with open('..\\ans.jpg','wb') as f1:
	f1.write(ans)

f.close()
f1.close()
```

### ox2 步骤

1. 图片操作

   打开图片显示的是残缺的二维码，接着进行对于图片的一些常规处理，发现存在许多图片，我们使用 *Stegsolve.jar* 进行分离后得到六张二维码片段

2. 图片拼接

   直接使用 Windows系统自带的图片编辑打开，手工进行选择粘贴，拼接后就可以得到完整的二维码图片

3. 扫码得flag

   扫码得到如下信息，看来还没有结束

   ```
   key:AutomaticKey 
   cipher:fftu{2028mb39927wn1f96o6e12z03j58002p}
   ```

4. [AutoKey Cipher （自动秘钥）密码解密](https://www.wishingstarmoye.com/ctf/autokey)

   `flag{2028ab39927df1d96e6a12b03e58002e}`


### 0x3 总结

1. 在提取数据的时候刚开始以为要分两种（前1位和前2位分开提取），但结果是两个值一起保存

2. 之所以将提取转换后的数据保存为图片，也是因为二进制转成ASCII时发现是 jpg 文件格式

3. python 脚本中内置函数进制转换

   | ↓      | 2进制           | 8进制           | 10进制           | 16进制           |
   | ------ | --------------- | --------------- | ---------------- | ---------------- |
   | 2进制  | `-`             | `bin(int(x,8))` | `bin(int(x,10))` | `bin(int(x,16))` |
   | 8进制  | `oct(int(x,2))` | `-`             | `oct(int(x,10))` | `oct(int(x,16))` |
   | 10进制 | `int(x,2)`      | `int(x,8)`      | `-`              | `int(x,16)`      |
   | 16进制 | `hex(int(x,2))` | `hex(int(x,8))` | `hex(int(x,10))` | `-`              |

   