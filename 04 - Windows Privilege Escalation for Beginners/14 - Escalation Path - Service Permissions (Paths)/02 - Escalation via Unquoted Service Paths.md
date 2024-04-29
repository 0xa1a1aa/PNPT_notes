Enumeration:

**PowerUp**:
```PowerShell
Invoke-AllChecks
```

or

WinPEAS

---

Example:
![[Pasted image 20240422095221.png]]

Run MSF module **multi/handler** with **meterpreter/reverser_tcp**

Create **meterpreter/reverser_tcp** executable with *msfvenom*
Name the executable **common.exe**.

>[!Note]
>Its important that the name is a part of the unquoted service path.
>In our case: `C:\Program Files\Unquoted Path Service\Common.exe`

Upload the executable and place it under `C:\Program Files\Unquoted Path Service\`

Start the service to run the exploit:
```cmd
sc start unquotedsvc
```