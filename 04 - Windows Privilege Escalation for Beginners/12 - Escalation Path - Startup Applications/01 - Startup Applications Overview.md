Check permissions of startup scripts.
```cmd
icacls.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
```

We are looking for permission (F) for users.

The permissions are explained here:
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/icacls#remarks

Exploit concept:
- Modify startup scripts to launch a reverse shell
- Wait for an administrative user to login
- Boom! Enjoy the administrative shell :)