In "Server Manager" -> "Tools" ->"Active Directory Users and Computers"
This opens a windows with the domain and all the associated OUs (e.g. Computers):
![[Pasted image 20240309172626.png]]

## Setup OU "Groups"

Create new OU "Groups":
![[Pasted image 20240309172812.png]]
![[Pasted image 20240309172847.png]]

Move all "Groups" that are per default under "Users" to the new OU "Groups" by drag'n drop.
![[Pasted image 20240309173022.png]]

## Setup user accounts

Create a 2nd administrator account. Right click and copy user "Administrator":
![[Pasted image 20240309173238.png]]
![[Pasted image 20240309173351.png]]
Password1234

Now we have created a new user account (Tony Stark):
![[Pasted image 20240309173510.png]]

Now we create a service account for SQL by copying the Administrator once more (this is not recommended btw):
![[Pasted image 20240309173702.png]]
This account will be used to run a SQL server (theoretically).
MYpassword123#

![[Pasted image 20240309173943.png]]In order to take note of the password some administrator put the password in the user description, because they think the description can only be seen by them, when in reality everyone can read the description. Right click -> Properties -> Description:
![[Pasted image 20240309174049.png]]

Create 2 new normal users for our windows workstations:
Right click -> New -> User:
![[Pasted image 20240309174419.png]]

**THEPUNISHER - frank**
![[Pasted image 20240309174519.png]]
Just set the same password as in the workstation.
Set "Password never expires":
![[Pasted image 20240309174627.png]]

**SPIDERMAN - peter**
Same process as above

We should now have the following users:
![[Pasted image 20240309174831.png]]

## Create a fileshare

We create a fileshare we can attack later on.

On the left sidebar of "Server Manager" click on "File and Storage Devices":
![[Pasted image 20240309175023.png]]
Then click on "Shares" and in the upper right corner on "TASKS" -> "New share":
![[Pasted image 20240309175143.png]]

Select "SMB Share Quick" -> (Share Location) Next -> Set Share name "hackme" + Next
![[Pasted image 20240309175331.png]]
-> (Other Settings) Next -> (Permissions) Next -> (Confirmation) Create
Done.

## Setup SQL service account fully

Open CMD as administrator:
![[Pasted image 20240309175919.png]]

Then type the following command:
```cmd
setspn -a HYDRA-DC/SQLService.MARVEL.local:60111 MARVEL\SQLService
```
Output:
![[Pasted image 20240309180305.png]]
Check if the service was setup correctly:
```cmd
setspn -T MARVEL.local -Q */*
```
Output:
![[Pasted image 20240309180534.png]]Ok: Existing SPN found. SQL Service was found.
Done.

## Setup Group Policy

Setup Group Policy for the entire domain.
![[Pasted image 20240309180807.png]]

Right click on "Marvel local" -> "Create a GPO in this domain...":
![[Pasted image 20240309180927.png]]

Set the GPO name -> OK:
![[Pasted image 20240309181038.png]]

Edit the newly created policy. Right Click -> Edit:
![[Pasted image 20240309181148.png]]

Navigate to "Policies" -> "Administrative Templates..." -> "Windows Components" -> "Microsoft Defender Antivirus":
![[Pasted image 20240309181529.png]]

Click on "Turn off Microsoft Defender Antivirus".
Then select "Enabled" and confirm with "Apply".
![[Pasted image 20240309181638.png]]

Finally right click on the new policy and set "Enforced":
![[Pasted image 20240309181751.png]]

![[Pasted image 20240309181838.png]]

## Check if the DC has an IP address

CMD "ipconfig":
![[Pasted image 20240309182045.png]]
Ok.
