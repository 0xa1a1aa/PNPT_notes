# Transfer Mimikatz to SPIDERMAN

On the kali machine enter the command `mimikatz`, this will change the directory to where the program is installed. Then change into the *x64* directory and start a python http server:
![[Pasted image 20240323113755.png]]

On SPIDERMAN download the files:
![[Pasted image 20240323113946.png]]

# Exploitation

Open an administrative command prompt and change to the download directory:
![[Pasted image 20240323114354.png]]

Run *mimikatz.exe*:
![[Pasted image 20240323114511.png]]

Acquire debug privileges:
List all privileges: `privilege::`
![[Pasted image 20240323114739.png]]

With `sekurlsa::` you can show the available options to enumerate credentials:
![[Pasted image 20240323114942.png]]

Show all login passwords with `sekurlsa::logonPasswords`:
This commands dumps a lot of content:
![[Pasted image 20240323115603.png]]
In the output there might also be cleartext passwords, like in our example.

