## Info

## Brute forcing

**Default Creds**

- https://www.cirt.net/passwords
- https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.csv
- https://github.com/scadastrangelove/SCADAPASS/blob/master/scadapass.csv

**User brute forcing**

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



## Refrences

- https://www.cirt.net/passwords
- https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.csv
- https://github.com/scadastrangelove/SCADAPASS/blob/master/scadapass.csv
- https://github.com/danielmiessler/SecLists/tree/master/Usernames
- https://github.com/danielmiessler/SecLists/tree/master/Usernames/top-usernames-shortlist.txt
- https://en.wikipedia.org/wiki/List_of_the_most_common_passwords
- 
