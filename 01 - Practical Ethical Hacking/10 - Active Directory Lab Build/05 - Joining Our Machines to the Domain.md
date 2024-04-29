In the Domain Controller set a static IP (10.0.0.18 in our case):
![[Pasted image 20240310105128.png]]

In both windows workstations set the Preferred DNS server to the IP of the DC:
![[Pasted image 20240310104945.png]]

Join the workstations by searching "domain" -> "Access work or school"
![[Pasted image 20240310105208.png]]

Hit "Connect":
![[Pasted image 20240310105237.png]]

Click on "Join this device to a local Active Directory domain":
![[Pasted image 20240310105352.png]]

Enter the domain name:
![[Pasted image 20240310105435.png]]

If the following popup appears, that means the DC was found and that we now have to login to the domain with the domain account info:
![[Pasted image 20240310105506.png]]
In our case the domain account info is:
administrator:P@\$\$w0rd!

On the next window we create a new user. In our case we will set this as our local administrator account (password is reset later):
![[Pasted image 20240310110109.png]]
-> Restart now

In the DC machine goto "Tools" -> "Active Directory Users and Computers":
![[Pasted image 20240310111141.png]]

When we navigate to "Computers" the 2 workstations should now show up:
![[Pasted image 20240310111217.png]]

Login to the workstations with the MARVEL\\administrator account (administrator:P@\$\$w0rd!).
Search for "users" and goto "Edit local users and groups":
![[Pasted image 20240310111729.png]]

Goto "Users" and right click on "Administrator" -> "Set Password...":
![[Pasted image 20240310112227.png]]
Set password to: Password1!

Then right click on "Administrator" -> "Properties":
![[Pasted image 20240310112411.png]]

Uncheck "Account is disabled" -> Apply:
![[Pasted image 20240310112510.png]]
Btw this is not best practice, see below.

https://learn.microsoft.com/en-us/windows/security/identity-protection/access-control/local-accounts:
>[!Info] Administrator
>The default local Administrator account is a user account for system administration. Every computer has an Administrator account (SID S-1-5-_domain_-500, display name Administrator). The Administrator account is the first account that is created during the Windows installation.
>\[...]
>Windows setup disables the built-in Administrator account and creates another local account that is a member of the Administrators group.

>[!Info] Security considerations
>Because the Administrator account is known to exist on many versions of the Windows operating system, **it's a best practice to disable the Administrator account when possible to make it more difficult for malicious users to gain access to the server or client computer**. You can rename the Administrator account.

## Administrator Group setup

Next goto "Groups" -> "Administrators":
![[Pasted image 20240310113139.png]]

Add a new administrator user:
Click "Add" -> type in "frank" and click on "Check Names". This will search and populate the field with the user "frank": 
![[Pasted image 20240310113258.png]]

Now the user "frank" is part of the administrator group. Hit Apply to confirm.
![[Pasted image 20240310113411.png]]

In the SPIDERMAN machine add both users to the Administrator group:
![[Pasted image 20240310114336.png]]
This will be user in an attack later on.

## Enable Network discovery

Open "Network": If the message "Network discovery is turned off..." shows up:
![[Pasted image 20240310113536.png]]

Right click on the message bar and select "Turn on network discovery and file sharing":
![[Pasted image 20240310113735.png]]

### Optional: DC doesn't show up in Network

If HYDRA-DC doesn't show up under "Network", make sure to turn on network discovery on the DC:
![[Pasted image 20240310133049.png]]
If you need to switch network profiles (e.g. Public -> Private) use the following powershell command:
```powershell
Get-NetConnectionProfile
Set-NetConnectionProfile -InterfaceAlias Ethernet -NetworkCategory Private
```


In the SPIDERMAN machine goto "This PC" -> "Computer" -> "Map network drive":
![[Pasted image 20240310120044.png]]

As folder choose the network share "hackme" of the DC.
Set "Connect using different credentials":
![[Pasted image 20240310120225.png]]

Login with administrator:P@\$\$w0rd!
Set "Remember my credentials":
![[Pasted image 20240310120502.png]]

Now the network share "hackme" shows up as the drive Z:
![[Pasted image 20240310120839.png]]