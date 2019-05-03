## Mine Twitter for Targeted Information with Twint

### Twitter as an OSINT Database

- #### Ref: [使用Buscador OSINT VM进行在线调查](https://null-byte.wonderhowto.com/how-to/use-buscador-osint-vm-for-conducting-online-investigations-0186611/)

### Data You'll Find on Twitter

- 关键信息的图像

- 公开提及的电话号码和个人信息

- 办公空间和私人住宅内的照片和视频

- 通过主题标签链接的来自其他人不同角度的图像和视频

- 关于一个人目前正在接受调查的任何主题范围的主要来源陈述

- 将目标挂起或花费时间

- 显示其他员工身份的公司或办公室照片

- 旅行记录和即将举行的个人活动

- #### Ref:  [如何使用SpiderFoot进行OSINT收集](https://null-byte.wonderhowto.com/how-to/use-spiderfoot-for-osint-gathering-0180063/)

### Slicing Through Data with Twint

- Twint配备了搜索过滤器，您可以通过这些过滤器以有用的方式组合以显示精确信息
- 通过正确组合搜索标记，您甚至可以搜索公开联系人以与目标帐户交换联系信息
- 虽然许多搜索工具使用您的Twitter帐户通过Twitter的API发出请求，但Twint却没有。这允许您绕过速率限制，通过代理查询，以及在您自己和正在研究的目标之间建立距离。由于能够快速生成文本和CSV文件以存档感兴趣的推文，Twint成为了一个很好的取证或调查Twitter工具
- Twint是一个Python库，可以编写脚本以执行更多自定义或复杂操作，是社交媒体中提取数据简单但强大的方法

### Install Twint

- [项目地址](https://github.com/twintproject/twint)

- `pip3 install --upgrade -e git+https://github.com/twintproject/twint.git@origin/master#egg=twint`

- 或者

  ```bash
  git clone https://github.com/twintproject/twint.git
  cd twint
  pip3 install -r requirements.txt
  ```

### View Twint's Options

`sudo twint -h`

### Grab a User's Recent Tweets

`sudo twint -u officialmcafee --since 2019-2-17`

### Locate Historical Evidence

`sudo twint -u officialmcafee -s "tax return" --since 2019-1-01`

`sudo twint -u officialmcafee -s "taxes" --since 2009-01-01 -o mcafeetax`

### Export Evidence & Metadata

`sudo twint -u officialmcafee -s "gun" --since 2018-01-01 --media -o mcafeeguns --csv `

### Collect Real-Time Data [^1]

`sudo twint --verified -s "arrested" --near 'Los Angeles' --since 2019-02-17 --images`

`sudo twint --verified -s "LAPD" --near 'Los Angeles' --since 2019-02-17 --videos `

`sudo twint --to officialmcafee --since 2019-01-01 -s help -o mcafeecontacts --csv ` [^2]

### Dig Deeper with Additional Searches

`sudo twint -u officialmcafee -s taxes --year 2018`

`sudo twint -u officialmcafee --location --since 2019-2-10 --media`

### Twitter Makes OSINT Easy [^3]



[原文](https://null-byte.wonderhowto.com/how-to/mine-twitter-for-targeted-information-with-twint-0193853/)

---

[^1]: Twint的优势在于能够提取有关Twitter用户报告的当前事件的信息。通过组合地理位置和主题标志，我们可以指定我们只想查看当前正在发生的关于我们当地区域中某些主题的帖子
[^2]: 将实时数据引入搜索的功能，即使用 **--to** 标志发送到此帐户的所有人，因此我们可以查看可能提供帮助的人或通过发送照片或问候语提供有关亲自遇到目标的其他线索
[^3]: 无论调查类型如何，来自社交媒体的数据都可以通过提供看似无穷无尽的信息来丰富您对事件的理解。从研究用户之间的互动到通过主题标签组织的媒体查找相同情况的替代视图，社交媒体上共享的信息应该是任何OSINT调查员收集的一部分