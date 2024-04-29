Install latest version of the bloodhound ingestor (https://github.com/dirkjanm/BloodHound.py):
```bash
sudo pip install bloodhound
```

---
## Neo4j

Start the *neo4j* database which is used by bloodhound:
```bash
sudo neo4j console
```
Success:
![[Pasted image 20240314173032.png]]
In the output we can see that it is listening on **localhost:7687** and a webinterface on http://localhost:7474/

**Webinterface:**
First time login:
Username: neo4j
PW: neo4j
![[Pasted image 20240314173411.png]]
Afterwards you have to change your password. New password: neo4j_

## Bloodhound

Run bloodhound:
```bash
sudo bloodhound
```
Type in the the neo4j credentials:
![[Pasted image 20240314173855.png]]

## Bloodhound Ingestor

Run the bloodhound ingestor (https://github.com/dirkjanm/BloodHound.py):
```bash
sudo bloodhound-python -d MARVEL.local -u frank -p Password1 -ns 10.0.0.18 -c all
```
Args:
`-d`: Domain to query
`-u`: Username
`-p`: Password
`-ns`: Nameserver
`-c`: Which information to collect

### Syntax error

If you get the following error:
![[Pasted image 20240314175235.png]]
Try the following command with the full path:
```bash
sudo /home/kali/.local/share/pipx/venvs/crackmapexec/bin/bloodhound-python -d MARVEL.local -u frank -p Password1 -ns 10.0.0.18 -c all
```
If that does not work try these too:
1. Run: `pipx ensurepath --force`
2. Run *pimpmykali* with the *%* option.

### Syntax error fixed

Successful output:
![[Pasted image 20240314183641.png]]
The data is stored in the current directory:
![[Pasted image 20240314183552.png]]

Upload the data in bloodhound:
![[Pasted image 20240314183916.png]]
Upload all the data from the ingestor:
![[Pasted image 20240314184029.png]]
Wait for the upload process to complete.

Under "Database Info" we can see that we have imported some objects like groups, computers, etc.:
![[Pasted image 20240314184233.png]]

Under "Analysis" we can do different analyses on the imported data. For example we can find all the domain admins:
![[Pasted image 20240314184450.png]]