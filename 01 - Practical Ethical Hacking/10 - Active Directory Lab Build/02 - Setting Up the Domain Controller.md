Setup steps:
1. Create new VM with Windows Server ISO
2. Create the administrator account (administrator:P@\$\$w0rd!):![[Pasted image 20240310110933.png]]
3. Rename machine:   ![[Pasted image 20240309132613.png]]
   ![[Pasted image 20240309132652.png]]
   ![[Pasted image 20240309132830.png]]
4. In the "Server Manager":
   ![[Pasted image 20240309133038.png]]
   On the wizard: Next -> "Role-based or feature-based installation"+Next -> HYDRA-DC + Next -> Set "Active Directory Domain Services" + Next -> Next -> Next -> Set automatic restart -> Install
   ![[Pasted image 20240309133325.png]]
   ![[Pasted image 20240309133428.png]]
5. Set "Promote Server do domain controller":   ![[Pasted image 20240309133655.png]]
6. Add new forest:   ![[Pasted image 20240309133800.png]]
7. Set password + Next -> (DNS) Next -> Wait for NetBIOS domain name to come up + Next -> (Paths) Next -> (Review Options) Next -> (Prerequisite Checks) Install ->(Installation) -> After installation VM will reboot
8. Login as MARVEL\\Administrator
9. In the "Server Manager" go again to "Manage"->"Add roles and features"
10. Click Next until "Server Roles". Add "Active Directory Certificate Services":
   ![[Pasted image 20240309134953.png]]
11. Click Next until "Confirmation". Set "Restart ..." and click Install.
12. After installation is done click "Configure ..."
    ![[Pasted image 20240309135205.png]]
13. (Credentials) Next ->(Role Services) Set "Certification Authority"    ![[Pasted image 20240309135313.png]] 
    -> Click next until "Validity Period". Set 99 years -> Next -> Configure
14. Reboot the VM. Done. 