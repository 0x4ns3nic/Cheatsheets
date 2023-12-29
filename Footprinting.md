https://buckets.grayhatwarfare.com
https://domain.glass




## FTP

- https://www.smartfile.com/blog/the-ultimate-ftp-commands-list
- https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes

`0x4ns3nic@htb[/htb]$ find / -type f -name ftp* 2>/dev/null | grep scripts`

```
0x4ns3nic@htb[/htb]$ nc -nv 10.129.14.136 21
0x4ns3nic@htb[/htb]$ telnet 10.129.14.136 21
0x4ns3nic@htb[/htb]$ openssl s_client -connect 10.129.14.136:21 -starttls ftp
```

`0x4ns3nic@htb[/htb]$ wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136`


## SMB

| SMB Version | Supported                           | Features                                                               |
| ----------- | ----------------------------------- | ---------------------------------------------------------------------- |
| CIFS        | Windows NT 4.0                      | Communication via NetBIOS interface                                    |
| SMB 1.0     | Windows 2000                        | Direct connection via TCP                                              |
| SMB 2.0     | Windows Vista, Windows Server 2008  | Performance upgrades, improved message signing, caching feature        |
| SMB 2.1     | Windows 7, Windows Server 2008 R2   | Locking mechanisms                                                     |
| SMB 3.0     | Windows 8, Windows Server 2012      | Multichannel connections, end-to-end encryption, remote storage access |
| SMB 3.0.2   | Windows 8.1, Windows Server 2012 R2 |                                                                        |
| SMB 3.1.1   | Windows 10, Windows Server 2016     | Integrity checking, AES-128 encryption                                 |

```
0x4ns3nic@htb[/htb]$ smbclient -N -L //10.129.14.128
0x4ns3nic@htb[/htb]$ smbclient //10.129.14.128/notes
0x4ns3nic@htb[/htb]$ smbmap -H 10.129.14.128
0x4ns3nic@htb[/htb]$ crackmapexec smb 10.129.14.128 --shares -u '' -p ''
```

`0x4ns3nic@htb[/htb]$ sudo nmap 10.129.14.128 -sV -sC -p139,445`


`0x4ns3nic@htb[/htb]$ rpcclient -U "" 10.129.14.128`

| Query                     | Description                                                        |
| ------------------------- | ------------------------------------------------------------------ |
| `srvinfo`                 | Server information.                                                |
| `enumdomains`             | Enumerate all domains that are deployed in the network.            |
| `querydominfo`            | Provides domain, server, and user information of deployed domains. |
| `netshareenumall`         | Enumerates all available shares.                                   |
| `netsharegetinfo <share>` | Provides information about a specific share.                       |
| `enumdomusers`            | Enumerates all domain users.                                       |
| `queryuser <RID>`         | Provides information about a specific user.                        |


`0x4ns3nic@htb[/htb]$ for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done`

`0x4ns3nic@htb[/htb]$ samrdump.py 10.129.14.128`

```
0x4ns3nic@htb[/htb]$ git clone https://github.com/cddmp/enum4linux-ng.git
0x4ns3nic@htb[/htb]$ cd enum4linux-ng
0x4ns3nic@htb[/htb]$ pip3 install -r requirements.
```

`0x4ns3nic@htb[/htb]$ ./enum4linux-ng.py 10.129.14.128 -A`


## NFS

| Version | Features                                                                                                                                                                                                                                                             |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `NFSv2` | It is older but is supported by many systems and was initially operated entirely over UDP.                                                                                                                                                                           |
| `NFSv3` | It has more features, including variable file size and better error reporting, but is not fully compatible with NFSv2 clients.                                                                                                                                       |
| `NFSv4` | It includes Kerberos, works through firewalls and on the Internet, no longer requires portmappers, supports ACLs, applies state-based operations, and provides performance improvements and high security. It is also the first version to have a stateful protocol. |

```
# Nmap
0x4ns3nic@htb[/htb]$ sudo nmap 10.129.14.128 -p111,2049 -sV -sC
0x4ns3nic@htb[/htb]$ sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049
# List shares
0x4ns3nic@htb[/htb]$ showmount -e 10.129.14.128
# Mount Share
0x4ns3nic@htb[/htb]$ mkdir target-NFS
0x4ns3nic@htb[/htb]$ sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
0x4ns3nic@htb[/htb]$ cd target-NFS
0x4ns3nic@htb[/htb]$ tree .
# List Contents with Usernames & Group Names
0x4ns3nic@htb[/htb]$ ls -l mnt/nfs/
0x4ns3nic@htb[/htb]$ ls -n mnt/nfs/
# Unmounting
0x4ns3nic@htb[/htb]$ cd ..
0x4ns3nic@htb[/htb]$ sudo umount ./target-NFS
```

## DNS

![](https://academy.hackthebox.com/storage/modules/27/tooldev-dns.png)

| DNS Record | Description                                                                                                                                                                                                                                       |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `A`        | Returns an IPv4 address of the requested domain as a result.                                                                                                                                                                                      |
| `AAAA`     | Returns an IPv6 address of the requested domain.                                                                                                                                                                                                  |
| `MX`       | Returns the responsible mail servers as a result.                                                                                                                                                                                                 |
| `NS`       | Returns the DNS servers (nameservers) of the domain.                                                                                                                                                                                              |
| `TXT`      | This record can contain various information. The all-rounder can be used, e.g., to validate the Google Search Console or validate SSL certificates. In addition, SPF and DMARC entries are set to validate mail traffic and protect it from spam. |
| `CNAME`    | This record serves as an alias. If the domain www.hackthebox.eu should point to the same IP, and we create an A record for one and a CNAME record for the other.                                                                                  |
| `PTR`      | The PTR record works the other way around (reverse lookup). It converts IP addresses into valid domain names.                                                                                                                                     |
| `SOA`      | Provides information about the corresponding DNS zone and email address of the administrative contact.                                                                                                                                            |



```
[!bash!]$ dig facebook.com @1.1.1.1
[!bash!]$ dig a www.facebook.com @1.1.1.1
[!bash!]$ dig -x 31.13.92.36 @1.1.1.1
[!bash!]$ dig any google.com @8.8.8.8
[!bash!]$ dig any cloudflare.com @8.8.8.8
[!bash!]$ dig txt facebook.com @1.1.1.1
[!bash!]$ dig mx facebook.com @1.1.1.1
0x4ns3nic@htb[/htb]$ dig soa www.inlanefreight.com
0x4ns3nic@htb[/htb]$ dig ns inlanefreight.htb @10.129.14.128
0x4ns3nic@htb[/htb]$ dig CH TXT version.bind 10.129.120.85
0x4ns3nic@htb[/htb]$ dig any inlanefreight.htb @10.129.14.128
0x4ns3nic@htb[/htb]$ dig axfr inlanefreight.htb @10.129.14.128
0x4ns3nic@htb[/htb]$ dig axfr internal.inlanefreight.htb @10.129.14.128
# Bruting
0x4ns3nic@htb[/htb]$ for sub in $(cat /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
# Automating
0x4ns3nic@htb[/htb]$ dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb
```'


## SMTP & IMAP / POP3

```
# SMTP
[!bash!]$ sudo nmap 10.129.14.128 -sC -sV -p25
[!bash!]$ sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v
# IMAP / POP3
0x4ns3nic@htb[/htb]$ sudo nmap 10.129.14.128 -sV -p110,143,993,995 -sC
0x4ns3nic@htb[/htb]$ curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd
```

## SNMP

```
0x4ns3nic@htb[/htb]$ snmpwalk -v2c -c public 10.129.14.128
0x4ns3nic@htb[/htb]$ onesixtyone -c /opt/useful/SecLists/Discovery/SNMP/snmp.txt 10.129.14.128
0x4ns3nic@htb[/htb]$ braa <community string>@<IP>:.1.3.6.*   # Syntax
0x4ns3nic@htb[/htb]$ braa public@10.129.14.128:.1.3.6.*
```

## MySql

```
0x4ns3nic@htb[/htb]$ sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*
0x4ns3nic@htb[/htb]$ mysql -u root -h 10.129.14.132
```

| Command                                              | Description                                                                                       |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `mysql -u <user> -p<password> -h <IP address>`       | Connect to the MySQL server. There should not be a space between the '-p' flag, and the password. |
| `show databases;`                                    | Show all databases.                                                                               |
| `use <database>;`                                    | Select one of the existing databases.                                                             |
| `show tables;`                                       | Show all available tables in the selected database.                                               |
| `show columns from <table>;`                         | Show all columns in the selected database.                                                        |
| `select * from <table>;`                             | Show everything in the desired table.                                                             |
| `select * from <table> where <column> = "<string>";` | Search for needed `string` in the desired table.                                                  |


-  https://dev.mysql.com/doc/refman/8.0/en/system-schema.html#:~:text=The%20mysql%20schema%20is%20the,used%20for%20other%20operational%20purposes


## MSSQL

**Default DB's**

| Default System Database | Description                                                                                                                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `master`                | Tracks all system information for an SQL server instance                                                                                                                                               |
| `model`                 | Template database that acts as a structure for every new database created. Any setting changed in the model database will be reflected in any new database created after changes to the model database |
| `msdb`                  | The SQL Server Agent uses this database to schedule jobs & alerts                                                                                                                                      |
| `tempdb`                | Stores temporary objects                                                                                                                                                                               |
| `resource`              | Read-only database containing system objects included with SQL server                                                                                                                                  |

| [mssql-cli](https://docs.microsoft.com/en-us/sql/tools/mssql-cli?view=sql-server-ver15) | [SQL Server PowerShell](https://docs.microsoft.com/en-us/sql/powershell/sql-server-powershell?view=sql-server-ver15) | [HeidiSQL](https://www.heidisql.com/) | [SQLPro](https://www.macsqlclient.com/) | [Impacket's mssqlclient.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py) |

```
0x4ns3nic@htb[/htb]$ sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248
msf6 auxiliary(scanner/mssql/mssql_ping) > set rhosts 10.129.201.248
0x4ns3nic@htb[/htb]$ python3 mssqlclient.py Administrator@10.129.201.248 -windows-auth
```

## Oracle TNS


```
#!/bin/bash

sudo apt-get install libaio1 python3-dev alien python3-pip -y
git clone https://github.com/quentinhardy/odat.git
cd odat/
git submodule init
git submodule update
sudo apt install oracle-instantclient-basic oracle-instantclient-devel oracle-instantclient-sqlplus -y
pip3 install cx_Oracle
sudo apt-get install python3-scapy -y
sudo pip3 install colorlog termcolor pycryptodome passlib python-libnmap
sudo pip3 install argcomplete && sudo activate-global-python-argcomplete
```

```
0x4ns3nic@htb[/htb]$ sudo nmap -p1521 -sV 10.129.204.235 --open
0x4ns3nic@htb[/htb]$ sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
0x4ns3nic@htb[/htb]$ ./odat.py all -s 10.129.204.235
# Connecting
0x4ns3nic@htb[/htb]$ sqlplus scott/tiger@10.129.204.235/XE
# error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory
0x4ns3nic@htb[/htb]$ sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
0x4ns3nic@htb[/htb]$ sqlplus scott/tiger@10.129.204.235/XE as sysdba
## Interaction
SQL> select * from user_role_privs;
SQL> select table_name from all_tables;
SQL> select * from user_role_privs;
SQL> select name, password from sys.user$;
0x4ns3nic@htb[/htb]$ ./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```

Linux	/var/www/html
Windows	C:\inetpub\wwwroot

## IPMI

```
0x4ns3nic@htb[/htb]$ sudo nmap -sU --script ipmi-version -p 623 ilo.inlanfreight.local
msf6 > use auxiliary/scanner/ipmi/ipmi_dumphashes 
msf6 > use auxiliary/scanner/ipmi/ipmi_version 

```

| Product         | Username      | Password                                                                  |
| --------------- | ------------- | ------------------------------------------------------------------------- |
| Dell iDRAC      | root          | calvin                                                                    |
| HP iLO          | Administrator | randomized 8-character string consisting of numbers and uppercase letters |
| Supermicro IPMI | ADMIN         | ADMIN                                                                     |
