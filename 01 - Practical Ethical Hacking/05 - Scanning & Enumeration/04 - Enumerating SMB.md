# SMB Intro

Wikipedia:
>Server Message Block (SMB) is a communication protocol used to share files, printers, serial ports, and miscellaneous communications between nodes on a network.

https://superuser.com/questions/694469/difference-between-netbios-and-smb:
> On Windows, _SMB_ can run **directly over TCP/IP without the need for _NetBIOS over TCP/IP_**. This will use, as you point out, **port 445**.
> 
 >Generally speaking, on other systems, you'll find services and applications using **port 139**. This, basically speaking, means that SMB is running with _NetBIOS over TCP/IP_, where, stack-wise, SMB is on top of NetBIOS if you are to imagine it with the OSI model.
 
![[hwu24.jpg]]

On a Windows-based system SMB runs directly on TCP/IP.
![[kJ1Am.jpg]]

# Enumeration w/ Metasploit

Metasploit:
```bash
msfconsole
```

Search all modules for smb:
```msf6 console
search smb
```

Grep modules that contain the string "version":
```msf6 console
grep version search smb
	111  auxiliary/scanner/smb/smb_version
```

To use the module type "use" followed by either the module number or the module name:
```msf6 console
use 111
use auxiliary/scanner/smb/smb_version
```

Show module info:
```msf6 console
info
```

Show module options:
```msf6 console
options
```

Set target
```msf6 console
set RHOSTS 10.0.0.10
```

![[Pasted image 20240305200833.png]]
=> Unix (Samba 2.2.1a)

# Enumeration SMBclient

List shares (2 possible command syntax)
```bash
smbclient -L \\10.0.0.10
smbclient -L \\\\10.0.0.10\\
```

Output:
![[Pasted image 20240305201612.png]]
There are 2 file shares:
- IPC$
- ADMIN$

Try connect to the ADMIN$ share:
```bash
smbclient '\\10.0.0.10\ADMIN$'
smbclient \\\\10.0.0.10\\ADMIN$
```

Nope:
![[Pasted image 20240305202007.png]]

Try connect to the IPC$ share:
```bash
smbclient '\\10.0.0.10\IPC$'
smbclient \\\\10.0.0.10\\IPC$
```

Success:
![[Pasted image 20240305202104.png]]

With "help" we can list all available commands:
![[Pasted image 20240305202156.png]]

Unfortunately we cannot list the files:
![[Pasted image 20240305202241.png]]