Locate bash:
```cmd
where /R c:\windows bash.exe
```

Locate wsl:
```cmd
where /R c:\windows wsl.exe
```

Spawning a bind shell as per PayloadsAllTheThings doesnt work.

Open the bash shell:
![[Pasted image 20240417081338.png]]

Checking the **history** we can find credentials:
![[Pasted image 20240417081610.png]]

Run psexec.py to with the creds to pop a system shell!
(alternative: smbexec.py, wmiexec.py)