# Workstations

Setup steps after OS install:
- Install VirtualBox Guest Additions on each workstation.
- Rename machines: Search "name" -> "Rename this PC"

## Workstation 1

Hostname: nebuchadnezzar
IP: 10.0.0.24

User: Neo
Pass: redpill
AD pass: r3dp1ll?

User: Administrator
Pass: redpill123.

## Workstation 2

Hostname: horizon
IP: 10.0.0.25

User: Trinity
Pass: cuteneo
AD pass: cu73n30!

User: Administrator
Pass: redpill123.

---

# Domain Controller

Domain name: matrix.internal

Hostname: zion
IP: 10.0.0.23

Domain Admin: Administrator
Pass: P4ssw0rd1!

Domain Admin: Morpheus
Pass: rage47M!

SQL Service Account: SQLService
Pass: !L00k@me

Share: `\\zion\sharelove`


Set SPN for SQLService Account:
https://learn.microsoft.com/en-us/windows/win32/ad/name-formats-for-unique-spns
```cmd
setspn -s SQL/zion.matrix.internal MATRIX\SQLService
```
Success:
![[Pasted image 20240527183511.png]]