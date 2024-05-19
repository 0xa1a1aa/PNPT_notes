# Enumeration

Nmap:
```bash
PORT      STATE SERVICE
9999/tcp  open  abyss
10000/tcp open  snet-sensor-mgmt
```

![[Pasted image 20240507175606.png]]
## Port 9999

![[Pasted image 20240507175739.png]]

## Port 10000

Brute-force dir:
![[Pasted image 20240507175916.png]]
=> At `/bin/`we can download an executable

file:
`brainpan.exe: PE32 executable (console) Intel 80386 (stripped to external PDB), for MS Windows, 5 sections`
# Initial foothold

# PrivEsc