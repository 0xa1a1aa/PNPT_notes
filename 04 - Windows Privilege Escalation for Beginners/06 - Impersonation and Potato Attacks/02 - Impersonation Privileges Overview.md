Szenario:
You popped a shell and now check what privileges you have:
(In a meterpreter shell you can use the command *getprivs*)
![[Pasted image 20240417083031.png]]
=> *SeImpersonatePrivilege* is enabled

>[!Info]
>See **PayloadsAllTheThings**  and https://github.com/gtworek/Priv2Admin for an overview of all the tokens and how to exploit them!

Interesting tokens to look out for:
- SeImpersonatePrivilege
- SeAssignPrimaryToken (potato attacks)