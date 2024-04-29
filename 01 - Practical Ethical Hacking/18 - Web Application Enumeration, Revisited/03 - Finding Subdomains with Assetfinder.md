Github: https://github.com/tomnomnom/assetfinder

---
# Installation

From the github page:
```bash
go get -u github.com/tomnomnom/assetfinder
```

This does not work anymore. We first have to create a module:
```bash
go mod init assetfinder
```
Afterwards we can run the command above.

![[Pasted image 20240326100552.png]]

# Usage

Search all assets related to a domain:
```bash
assetfinder <domain>
```

Search for subdomains only:
```bash
assetfinder -subs-only <domain>
```

