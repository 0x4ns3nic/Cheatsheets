## Info

- Modules Path:`/usr/share/metasploit-framework/modules`
- Type of modules
  - auxiliary
  - encoders
  - evasion
  - exploits
  - nops
  - payloads
  - post
- Plugins: `ls /usr/share/metasploit-framework/plugins/`
- Scripts: `ls /usr/share/metasploit-framework/scripts/`
- Tools: `ls /usr/share/metasploit-framework/tools/`

Update metasploit: `sudo apt update && sudo apt install metasploit-framework`

![](https://academy.hackthebox.com/storage/modules/39/S04_SS03.png)

Syntax: `<No.> <type>/<os>/<service>/<name>`

| Type | Description |
| --- | --- |
| Auxiliary | Scanning, fuzzing, sniffing, and admin capabilities. Offer extra assistance and functionality. |
| Encoders | Ensure that payloads are intact to their destination. |
| Exploits | Defined as modules that exploit a vulnerability that will allow for the payload delivery. |
| NOPs | (No Operation code) Keep the payload sizes consistent across exploit attempts. |
| Payloads | Code runs remotely and calls back to the attacker machine to establish a connection (or shell). |
| Plugins | Additional scripts can be integrated within an assessment with msfconsole and coexist. |
| Post | Wide array of modules to gather information, pivot deeper, etc. |

The Service tag refers to the vulnerable service that is running on the target machine. 

Finally, the Name tag explains the actual action that can be performed using this module created for a specific purpose.

## Important Commands

| Command | Functionality |
| --- | --- |
| help search | Display help for the search command. |
| search [<options>] [<keywords>] | Search for modules based on keywords and options. |
| use <module> | Select a specific module for use. |
| show options | Display options and their current settings. |
| set <option> <value> | Set the value of a module option. |
| setg <option> <value> | Set a global (persistent) value for an option. |
| info | Display detailed information about the selected module. |
| run | Execute the selected module against the target. |
| sessions | List all active sessions. |
| sessions -i <session_id> | Interact with a specific session. |
| shell | Open a system shell on the target after successful exploitation. |
| background | Background the current session. |
| exit | Exit the msfconsole. |

Targets are unique operating system identifiers taken from the versions of those specific operating systems which adapt the selected exploit module to run on that particular version of the operating system. 

`msf6 > show targets`

`msf6 exploit(windows/browser/ie_execcommand_uaf) > info`

`msf6 exploit(windows/browser/ie_execcommand_uaf) > options`

## Payloads

1. Singles: These payloads contain both the exploit and the entire shellcode necessary for the task. They are stable and straightforward to use, but their size can sometimes cause issues with certain exploits. Singles are executed on the target system independently and provide immediate results, such as adding a user or starting a process.
2. Stagers: Stagers are smaller payloads designed to set up a network connection between the attacker and the victim. They wait on the attacker's machine and connect to the victim host once the Stage payload completes its run on the target system. Stagers are reliable and used to ensure successful communication between the attacker and the compromised system.
3. Stages: Stages are payload components that are downloaded by Stager modules. They offer advanced features with no size limits, including tools like Meterpreter and VNC Injection. Stages are beneficial for handling large payloads and overcoming issues with data transfer. They follow a process where the Stager receives the middle Stager, and the middle Stager performs a full download, making them more efficient for certain tasks.
