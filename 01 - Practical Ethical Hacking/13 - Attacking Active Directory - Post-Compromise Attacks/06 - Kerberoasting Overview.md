Explanation:
https://www.sentinelone.com/cybersecurity-101/what-is-kerberoasting-attack/

Attack steps:
1. In a Kerberoasting attack, the attacker first obtains the necessary permissions to request service tickets from the Kerberos authentication service. This can be done by compromising the account of a legitimate user who has the appropriate permissions.
2. In a Kerberoasting attack, an attacker obtains a large number of service tickets from the Kerberos authentication service and then uses these tickets to try to crack the passwords of the accounts associated with those tickets. = Service Principal Names (SPNs)

---

When we have a domain account, we can request a Ticket-Granting-Ticket (TGT) from the Key-Distribution-Center (KDC).
With the TGT we can request a Service Ticket from a Ticket-Granting-Service (TGS).
Parts of the Service Ticket is encrypted with a service password. If we are able to crack the password we can take over the AD account associated with that service, if any.

---

Tools:
- GetUserSPNs.py
- hashcat -n 13100