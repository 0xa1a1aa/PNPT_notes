# ARP Scanning

## netdiscover

active/passive ARP reconnaissance tool

```bash
sudo netdiscover -i eth1 -r 10.0.0.0/24
```

Example (from cmd above):
![[Pasted image 20240302220754.png]]

Wireshark capture:
![[Pasted image 20240302220849.png]]
So all what **netdiscover** does is sending a ARP requests to the broadcast address for the entire subnet.

## arp-scan

Send ARP requests to target hosts and display responses
```bash
sudo arp-scan -I eth0 10.0.0.1
```

Example:
![[Pasted image 20240302215426.png]]

Wireshark capture:
![[Pasted image 20240302215607.png]]
So all what **arp-scan** does is sending an ARP request to the broadcast address.

## arp-fingerprint

Fingerprint a system using ARP
```bash
sudo arp-fingerprint -o "-I eth1" -l
```

Example:
![[Pasted image 20240302220041.png]]

Wireshark capture:
![[Pasted image 20240302220217.png]]
-- snip --
![[Pasted image 20240302220442.png]]

So what **arp-fingerprint** does is sending a ARP requests to the broadcast address for the entire subnet.
We can also see some Malformed ARP packets. This is the tool trying to fingerprint the OS?

## arping

Ping a host via ARP requests. Can be used if the host does not respond to normal ping (ICMP) requests.
```bash
sudo arping -i eth1 -c 3 10.0.0.10
```

Example:
![[Pasted image 20240302221141.png]]

Wireshark capture:
![[Pasted image 20240302221210.png]]

So all what **arping** does is sending an ARP request to the broadcast address for the specified IP address.
# Nmap

```bash
# Nmap 7.94SVN scan initiated Tue Mar  5 16:00:23 2024 as: nmap -v -p- -sV -sC -oN kioptrix.nmap 10.0.0.10
Nmap scan report for 10.0.0.10
Host is up (0.00013s latency).
Not shown: 65529 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 2.9p2 (protocol 1.99)
|_sshv1: Server supports SSHv1
| ssh-hostkey: 
|   1024 b8:74:6c:db:fd:8b:e6:66:e9:2a:2b:df:5e:6f:64:86 (RSA1)
|   1024 8f:8e:5b:81:ed:21:ab:c1:80:e1:57:a3:3c:85:c4:71 (DSA)
|_  1024 ed:4e:a9:4a:06:14:ff:15:14:ce:da:3a:80:db:e2:81 (RSA)
80/tcp    open  http        Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
|_http-title: Test Page for the Apache Web Server on Red Hat Linux
|_http-server-header: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
| http-methods: 
|   Supported Methods: GET HEAD OPTIONS TRACE
|_  Potentially risky methods: TRACE
111/tcp   open  rpcbind     2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100024  1          32768/tcp   status
|_  100024  1          32768/udp   status
139/tcp   open  netbios-ssn Samba smbd (workgroup: LMYGROUP)
443/tcp   open  ssl/https   Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_ssl-date: 2024-03-05T20:00:45+00:00; +5h00m01s from scanner time.
|_http-server-header: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Issuer: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Public Key type: rsa
| Public Key bits: 1024
| Signature Algorithm: md5WithRSAEncryption
| Not valid before: 2009-09-26T09:32:06
| Not valid after:  2010-09-26T09:32:06
| MD5:   78ce:5293:4723:e7fe:c28d:74ab:42d7:02f1
|_SHA-1: 9c42:91c3:bed2:a95b:983d:10ac:f766:ecb9:8766:1d33
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_DES_192_EDE3_CBC_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|     SSL2_RC4_128_WITH_MD5
|     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|     SSL2_DES_64_CBC_WITH_MD5
|_    SSL2_RC4_64_WITH_MD5
|_http-title: 400 Bad Request
32768/tcp open  status      1 (RPC #100024)
MAC Address: 08:00:27:09:29:F7 (Oracle VirtualBox virtual NIC)

Host script results:
|_clock-skew: 5h00m00s
| nbstat: NetBIOS name: KIOPTRIX, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   KIOPTRIX<00>         Flags: <unique><active>
|   KIOPTRIX<03>         Flags: <unique><active>
|   KIOPTRIX<20>         Flags: <unique><active>
|   MYGROUP<00>          Flags: <group><active>
|_  MYGROUP<1e>          Flags: <group><active>
|_smb2-time: Protocol negotiation failed (SMB2)

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Mar  5 16:00:44 2024 -- 1 IP address (1 host up) scanned in 20.99 seconds
```

Try different scan types too!
```
nmap -T4 -p- -A
```