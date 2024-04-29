FTP anonymous login

FTP has 2 modes: ascii, binary:
To download larger files use binary

https://www.coreftp.com/docs/web1/Ascii_vs_Binary_transfers.htm

Recursive download of folders:
Use the second cmd if the username or password contain special chars.
```bash
wget -r ftp://user:pass@server.com/
wget -r --user="user$$login" --password="Pa$$wo|^D" ftp://server.com/
```

Linux tool to read .mdb files:
`mdb-sql <file.mdb>`

Linux tool to read .pst files:
`readpst <file.pst>`

The mdb file contains credentials.
use engineer pw to unzip the pst file, which contains an email containing a pw.

# Summary - Exploitation path

1) FTP anonymous login
2) access sensitive files
3) extract credentials from sensitive files 
4) Use credentials to login into another service (telnet)