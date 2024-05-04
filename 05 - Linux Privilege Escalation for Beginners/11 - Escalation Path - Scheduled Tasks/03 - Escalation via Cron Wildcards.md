**Example:**
*compress.sh* is run as a privileged cronjob:
![[Pasted image 20240504101314.png]]

Abuse wildcard by creating files with names set to tar arguments, e.g. --checkpoint=1:
![[Pasted image 20240504101703.png]]
Note: between "sh\\ runme.sh" is an empty space, cause runme.sh would be the 3rd argument passed to tar.
