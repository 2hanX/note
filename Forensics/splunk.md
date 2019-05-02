## Digital Forensics , Part 11 (Using Splunk)

### what is Splunk [^1]

### [Downloading Splunk](http://www.splunk.com/en_us/download/sem.html)

### Logging into Splunk

### Using Splunk （Demo）

- Add Data > Local Event Logs > system & Application & Security 

### Gathering the Data

- click Review tab
- click on the "Patterns" tab and Splunk will work to find patterns in the data and categorize them.
- ...

### Stop Splunk Server

- cmd : `compmgmt.msc`
- service: `Splunkd Service`
- stop

---

### Refer

- [Splunk Tutorial](https://docs.splunk.com/Documentation/Splunk/7.2.6/SearchTutorial/WelcometotheSearchTutorial)



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-11-using-splunk-0167012/)

---

[^1]: 在最基本的层面上，Splunk能够收集系统生成的所有数据并将其编入索引以进行搜索。Splunk最初是为系统管理而开发的，也可以成为数字取证的绝佳工具。在进行取证检查时，我们通常不会在受损系统上安装Splunk。在这些情况下，我们可以从受感染的系统收集数据，然后通过Splunk运行以进行分析