`L3pr3ch4un@htb[/htb]**$** nmap -p 80,443,8000,8080,8180,8888,1000 --open -oA web_discovery -iL scope`

`L3pr3ch4un@htb[/htb]**$** sudo nmap --open -sV 10.129.201.50`

`L3pr3ch4un@htb[/htb]**$** eyewitness --web -x web_discovery.xml -d inlanefreight_eyewitness`

`L3pr3ch4un@htb[/htb]**$** cat web_discovery.xml | ./aquatone -nmap`
