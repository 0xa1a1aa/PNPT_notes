Show service information about windows defender:
```shell
sc query windefend
```

List information about all running services:
```shell
sc queryex type= service
```

Show information about the firewall:
```shell
netsh advfirewall firewall dump
netsh firewall show state
```

