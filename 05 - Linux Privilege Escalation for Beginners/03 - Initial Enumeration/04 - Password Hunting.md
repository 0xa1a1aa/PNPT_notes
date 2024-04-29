Grep for passwords:
```bash
grep --color=auto -rnw '/' -ie 'PASSWORD' --color=always 2>/dev/null
```

Show filenames that contain "password":
```bash
locate password
locate pass
locate pwd
```

Search for ssh keys:
```bash
find / -name authorized_keys 2>/dev/null
find / -name id_rsa 2>/dev/null
```