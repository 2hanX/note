### Forensic Artifacts

| Directory   | Description                              |
| ----------- | ---------------------------------------- |
| `/bin`      | The essential command binaries           |
| `/boot`     | Files required for the system bootloader |
| `/dev`      | Device files                             |
| `/etc`      | System configuration files               |
| `/home`     | Home directories                         |
| `/lib`      | Shared libraries and kernel modules      |
| `/media`    | Mount points for removable media         |
| `/opt`      | Add-on application packages              |
| `/root`     | Root user home directory                 |
| `/sbin`     | System binaries                          |
| `/tmp`      | Temporary files                          |
| `/var/logs` | Centralized repository of log files      |

### Special Artifacts

| Artifacts                    | Location                                        |
| ---------------------------- | ----------------------------------------------- |
| User profile                 | `/home/$USER`                                   |
| System and Application logs  | `/etc`                                          |
| Operating system information | `/etc/os-release`                               |
| Operating system install     | `/root/install.log`                             |
| Host/ Computer name          | `/etc/hostname`                                 |
| IP address, DNS              | `/var/log`                                      |
| Time Zone Information        | `/etc/timezone`                                 |
| User login History           | `/var/log/auth.log`                             |
| Recently Accessed files      | `/home/username/local/share/recently-used.xbel` |
| Command History              | `$HOME/.bash_history`                           |

