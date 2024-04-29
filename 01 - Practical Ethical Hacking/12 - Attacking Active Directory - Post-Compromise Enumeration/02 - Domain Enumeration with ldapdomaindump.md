ldapdomaindump:
```bash
sudo /usr/bin/ldapdomaindump ldaps://10.0.0.18 -u 'MARVEL\frank' -p Password1
```
Args:
`ldaps://10.0.0.18`: IP address of DC
`-u`: Username
`-p`: Password

>[!Note]
>Use the full path */usr/bin/ldapdomaindump* or the command will fail with the error 'ValueError: unsupported hash type MD4'

Output:
![[Pasted image 20240314172048.png]]

All the LDAP information was stored in the current directory:
![[Pasted image 20240314172111.png]]

If we open *domain_users_by_group.html* in firefox, we can see the high value targets aka. domain/enterprise admins  :)
![[Pasted image 20240314172326.png]]