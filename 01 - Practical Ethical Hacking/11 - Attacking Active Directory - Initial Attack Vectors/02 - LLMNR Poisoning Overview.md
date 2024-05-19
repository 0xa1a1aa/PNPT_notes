TCM Blog: https://tcm-sec.com/llmnr-poisoning-and-how-to-prevent-it/
Other blog: https://redfoxsec.com/blog/what-is-llmnr-poisoning-and-how-to-avoid-it/

>[!Info] LLMNR protocol
>LLMNR stands for **Link-Local Multicast Name Resolution** and is used by Windows to resolve the names of neighbouring computers without using a domain name system (DNS) server. LLMNR works by sending out multicast queries over local networks asking if any specific computers with certain names exist and whether any have responded with their IP addresses when queried by LLMNR.

>[!Info] LLMNR poisoning
>LLMNR poisoning is a **man-in-the-middle (MITM) attack** that exploits this protocol. An attacker sends out a fake LLMNR response pretending to be from the computer with which the name was requested; if accepted by its recipient it can then be directed towards a malicious website or login page where sensitive data could be stolen by its author.

Attack scenario:
![[Pasted image 20240312123307.png]]

Attack tool:
**Responder**
https://github.com/SpiderLabs/Responder

- Responder will impersonate services using LLMNR, NBT-NS, mDNS, WPAD protocols, capturing credentials (usually NTLMv2 Challenge/Response) when a user attempts to authenticate against the spoofed services.
- Attempts can be made to downgrade to NetNTLMv1 or disable ESS for easier credential cracking.

>[!Note]
>A good time of the day to launch a LLMNR poisoning attack is early in the morning or after lunch, when people login into their computers.
