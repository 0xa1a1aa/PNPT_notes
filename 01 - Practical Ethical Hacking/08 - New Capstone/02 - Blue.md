# Information Gathering

SMB version:
![[Pasted image 20240306193316.png]]

Google:
![[Pasted image 20240306194112.png]]
=> EternalBlue exploit

# Scanning & Enumeration

Nmap all ports:
```bash
# Nmap 7.94SVN scan initiated Wed Mar  6 09:34:10 2024 as: nmap -v -p- -oN blue_all.nmap 10.0.0.13
Nmap scan report for 10.0.0.13
Host is up (0.00039s latency).
Not shown: 65527 closed tcp ports (conn-refused)
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49156/tcp open  unknown

Read data files from: /usr/bin/../share/nmap
# Nmap done at Wed Mar  6 09:36:12 2024 -- 1 IP address (1 host up) scanned in 121.86 seconds
```

Nmap open ports:
```bash
# Nmap 7.94SVN scan initiated Wed Mar  6 09:38:18 2024 as: nmap -v -p 135,139,445,49152,49153,49154,49155,49156 -sV -sC -oN blue_open.nmap 10.0.0.13
Nmap scan report for 10.0.0.13
Host is up (0.00045s latency).

PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Ultimate 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49156/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: WIN-845Q99OO4PP; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| nbstat: NetBIOS name: WIN-845Q99OO4PP, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:7e:f9:be (Oracle VirtualBox virtual NIC)
| Names:
|   WIN-845Q99OO4PP<00>  Flags: <unique><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WIN-845Q99OO4PP<20>  Flags: <unique><active>
|   WORKGROUP<1e>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|_  \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 7 Ultimate 7601 Service Pack 1 (Windows 7 Ultimate 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1
|   Computer name: WIN-845Q99OO4PP
|   NetBIOS computer name: WIN-845Q99OO4PP\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2024-03-06T03:39:19-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2024-03-06T08:39:19
|_  start_date: 2024-03-06T08:32:02
|_clock-skew: mean: 1h40m01s, deviation: 2h53m12s, median: 1s

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Mar  6 09:39:22 2024 -- 1 IP address (1 host up) scanned in 64.26 seconds
```

## SMB enumeration

![[Pasted image 20240306193716.png]]
# Exploitation

Metasploit comes up with multiple modules when searching for "eternalblue": 
![[Pasted image 20240306194310.png]]
=> Lets start with the first module

![[Pasted image 20240306194658.png]]