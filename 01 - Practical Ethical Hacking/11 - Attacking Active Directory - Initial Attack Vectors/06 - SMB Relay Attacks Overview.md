Resources:
- TCM blog: https://tcm-sec.com/smb-relay-attacks-and-how-to-prevent-them/
- Red Team Notes: https://www.ired.team/offensive-security/lateral-movement/lateral-movement-via-smb-relaying-by-abusing-lack-of-smb-signing
- https://viperone.gitbook.io/pentest-everything/everything/everything-active-directory/adversary-in-the-middle/smb-relay
---
**Viperone:**
> A SMB relay attack is where an attacker captures a users NTLM hash and relays its to another machine on the network. Masquerading as the user and authenticating against SMB to gain shell or file access.

**TCM blog:**
Requirements for an attack:
- The target must not enforce or enable SMB signing.
- For valuable outcomes, the relayed user’s credentials must have local admin status on the machine.
- You cannot relay credentials to the same machine they were captured from.
- Note: By default, all Windows workstations (non-servers) have SMB signing either disabled or not enforced.
- Another note: Since these credentials undergo relaying, password strength becomes irrelevant.

