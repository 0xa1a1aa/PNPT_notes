Crack the NTLMv2 hash with hashcat.
We can grep "NTLM" in the help output of hashcat, to see which NTLM hashes are supported:
```bash
hashcat --help | grep NTLM
```
Results:
![[Pasted image 20240312133800.png]]
In our case we got a NTLMv2 hash, thus we will use the hash mode 5600.

Cracking the NTLMv2 hash that is stored in the file "ntlmv2_hash.txt":
```bash
hashcat -m 5600 ntlmv2_hash.txt /usr/share/wordlists/rockyou.txt
```
Success:
![[Pasted image 20240312134451.png]]

Hashcat will log the results. If we provide the above command with the argument `--show`, hashcat shows us any previously cracked passwords:
![[Pasted image 20240312134627.png]]

Optional:
Hashcat rule (password list mutator):
https://github.com/stealthsploit/OneRuleToRuleThemStill