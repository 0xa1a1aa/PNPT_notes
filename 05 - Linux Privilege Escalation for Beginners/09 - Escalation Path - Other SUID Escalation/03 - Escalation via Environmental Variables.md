Print environment variables:
```bash
env
```

Search for SUID binaries:
```bash
find / -uid 0 -perm -4000 -type f 2>/dev/null
```

Use `strings` on a binary.
If a binary calls a binary just with its name and not the absolute path, we can hijack the call by creating our own binary and update the PATH variable, such that our binary is called.
Our binary spawns a shell, which is a root shell when called by the SUID binary.

---

If the SUID calls the binary with the absolute path we can exploit it by creating a bash function and export it:
![[Pasted image 20240429204825.png]]