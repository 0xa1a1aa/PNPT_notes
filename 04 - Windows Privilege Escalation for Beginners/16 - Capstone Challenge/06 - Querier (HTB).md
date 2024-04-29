# Initial foothold

SMB share **Reports** download xlsm file
use `binwalk -e` to extract the xlsm file contents
run `strings` on the file  *vbaProject.bin*: uid=reporting, pwd=...

Connect with the credentials to the mssql database:
![[Pasted image 20240422161840.png]]

Try to get a cmd shell:
![[Pasted image 20240422161951.png]]
=> nope

Host an SMB server:
![[Pasted image 20240422162244.png]]

In the SQL prompt issue this command:
![[Pasted image 20240422162315.png]]
=> this will send an SMB request and send the NTLMv2 hash shown above.

**Resources:**
Capturing MSSQL Credentials - [https://medium.com/@markmotig/how-to-capture-mssql-credentials-with-xp-dirtree-smbserver-py-5c29d852f478](https://medium.com/@markmotig/how-to-capture-mssql-credentials-with-xp-dirtree-smbserver-py-5c29d852f478)

Crack the NTLMv2 hash with john:
![[Pasted image 20240422162646.png]]
=> corporate568

Access the SQL database with the new credentials:
![[Pasted image 20240422163105.png]]

Now we can enable the cmd shell:
![[Pasted image 20240422163231.png]]

Upload netcat:
![[Pasted image 20240422163455.png]]

Pop a reverse shell:
![[Pasted image 20240422163707.png]]

![[Pasted image 20240422163625.png]]

---
# PrivEsc

Upload PowerUp and execute Invoke-AllChecks:
![[Pasted image 20240422163929.png]]

![[Pasted image 20240422164033.png]]

Query service info:
![[Pasted image 20240422164134.png]]

Change service binary path to pop a reverse shell:
![[Pasted image 20240422164252.png]]

![[Pasted image 20240422164332.png]]

Stop and start the service:
![[Pasted image 20240422164501.png]]

Enjoy the system shell:
![[Pasted image 20240422164426.png]]


Alternative way:
GPP password to get administrator password