Start powershell with script execution enabled:
```cmd
powershell -ep bypass
```

In powershell enter:
```cmd
Get-Acl -Path hklm:\System\CurrentControlSet\services\regsvc | fl
```

If we have FullControl as INTERACTIVE user we can exploit this:
![[Pasted image 20240420095044.png]]