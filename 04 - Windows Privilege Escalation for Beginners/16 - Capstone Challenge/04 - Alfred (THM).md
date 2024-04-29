Jenkins port 8080:
Login: admin:admin

Build History -> Project -> Configure -> Build -> execute command

Command to execute:
upload and execute ->
https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1

---
PrivEsc

Chech users privs:
```cmd
whoami /priv
```
=> SeImpersonatePrivilege

impersonate attack