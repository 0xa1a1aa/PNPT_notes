Search for SUID binaries:
```bash
find / -uid 0 -perm -4000 -type f 2>/dev/null
```

Use `strace` on a SUID binary to see what system calls are made.
Look for interesting syscalls like access/open that failed for files we can hijack:
![[Pasted image 20240429201442.png]]

Create and compile the */home/user/.config/libcalc.so* shared object file to pop a root shell.