# Enumeration

Ports: 22, 80

## HTTP

Dir brute-forcing:

Login mask:
http://10.10.120.28/content/as/index.php

Discovered backup file:
`/content/inc/mysql_backup/mysql_bakup_20191129023059-1.5.1.sql`

MySQL backup file contains MD5 hash: 42f749ade7f9e195bf475f37a44cafcb
```bash
hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
```
=> Password123
=> Username: either "admin" or "manager"

Login with manager:Password123 successful!

Database info:
![[Pasted image 20240505103004.png]]
# Initial foothold

Data import *rshell.phtml* reverse shell. Click on filename after upload to trigger the reverse shell:
![[Pasted image 20240505104157.png]]

![[Pasted image 20240505104253.png]]

# PrivEsc

Interesting users (/etc/passwd):
- itguy
- guest-3myc2b

Readable home directory:
![[Pasted image 20240505104449.png]]

User flag `/home/itguy/user.txt`: THM{63e5bce9271952aad1113b6f1ac28a07}

Interesting files:
![[Pasted image 20240505105008.png]]

MySQL database connect:
![[Pasted image 20240505105420.png]]
=> Nothing interesting

## LinPEAS


![[Pasted image 20240505110127.png]]

itguy is in sudo group:
![[Pasted image 20240505110207.png]]

backup.pl runs `/etc/copy.sh` in a new shell:
![[Pasted image 20240505111556.png]]

We can write and thus modify `/etc/copy.sh` spawn a root shell:
![[Pasted image 20240505112901.png]]

Root flag: `THM{6637f41d0177b6f37cb20d775124699f}`