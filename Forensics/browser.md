## Digital Forensics , Part 13 (Browser Forensics)

### Internet Explorer [^1]

- #### Windows 2000 & XP

  - `％systemdir％\ Documents and Settings \％username％\ Local Settings \ Temporary Internet Files \ Content.ie5`
  - `％systemdir％\ Documents and Settings \％username％\ Cookies`
  - `％systemdir％\ Documents and Settings \％username％\ Local Settings \ History \ history.ie5`

- #### Windows Vista & 7

  - `％systemdir％\ Users \％username％\ AppData \ Local \ Microsoft \ Windows \ Temporary Internet Files \`
  - `％systemdir％\ Users \％username％\ AppData \ Local \ Microsoft \ Windows \ Temporary Internet Files \ Low \`

### Mozilla Firefox

- #### Windows XP

  - `C:\Documents and Settings\<username>\Application Data\Mozilla\Firefox\Profiles\<profile folder>\places.sqlite`

- #### Windows Vista & 7

  - `C:\Users\<user>\AppData\Roaming\Mozilla\Firefox\Profiles\<profile folder>\places.sqlite`

- #### GNU/Linux

  - `/home/<user>/.mozilla/firefox/<profile folder>/places.sqlite`

- #### Mac OS X

  - `/Users/<user>/Library/Application Support/Firefox/Profiles/default.lov/places.sqlite`

### Using SQLite to Find Browser Evidence [^2]

kali : `sqlitebrower`

### Load the Database File into the SQLite Browser

### Querying the Database

`SELECT * FROM moz_inputhistory`

### Finding Specific User Input

`SELECT * FROM moz_inputhistory WHERE input like '%tor%'`



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-13-browser-forensics-0168280/)

---

[^1]: 探讨取证调查员在其Web浏览器中可以找到有关嫌疑人活动的信息的位置和内容。请务必注意，此信息因操作系统和浏览器而异
[^2]: SQLite现在被许多需要小型轻量级关系数据库的浏览器，应用程序和移动设备使用。由于其轻巧的特性，它在移动设备和移动应用程序中越来越受欢迎