**Best defensive solution**: Disable LLMNR, NBT-NS, mDNS

In the windows server this can be done by creating/editing a group policy and "Turn off multicast name resolution" under "Administrative Template" -> "Network" -> "DNS Client":
![[Pasted image 20240312135620.png]]

If LLMNR can't turned off, **the next best defensive solution** is to implement network access control and require strong user passwords.
