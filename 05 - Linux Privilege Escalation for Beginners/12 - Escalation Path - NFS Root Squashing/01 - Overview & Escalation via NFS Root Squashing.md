Check for any folder with *no_root_squash* attribute:
```bash
cat /etc/exports
```
Such a folder can be mounted remotely.

Check remotely with:
```bash
showmount -e <ip>
```