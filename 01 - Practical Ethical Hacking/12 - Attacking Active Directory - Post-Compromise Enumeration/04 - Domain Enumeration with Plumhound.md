Plumhound github: https://github.com/PlumHound/PlumHound

---
Install dependencies:
```bash
cd PlumHound
sudo pip3 install -r requirements.txt
```

Run PlumHound:
```bash
sudo python3 PlumHound.py -p neo4j_ --easy
```
Args:
`--easy`: Test Database Connection, Returns Domain Users to stdout.
`-p`: Neo4J Database Password

>[!Note]
>Bloodhound and Neo4J must be running for PlumHound to work!

PlumHound easy output:
![[Pasted image 20240315095940.png]]

Run default tasks:
```bash
sudo python3 PlumHound.py -p neo4j_ -x tasks/default.tasks --op /home/kali/courses/PNPT/AD/post_compromise_enumeration/plumhound/default_tasks
```
Args:
`--op`: Output Path for Reports (per default it creates the *reports* folder in the current directory)

This command will create a lot of files in the output directory. We can open *index.html* in firefox for the full report view:
![[Pasted image 20240315101420.png]]