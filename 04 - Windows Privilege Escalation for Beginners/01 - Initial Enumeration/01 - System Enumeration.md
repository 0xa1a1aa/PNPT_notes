General system info:
```shell
systeminfo
```

Just output *name*, *version*, *type*: 
```shell
systeminfo | findstr /b /c:"OS Name" /c:"OS Version" /c:"System Type"
```

List installed patches:
```shell
wmic qfe
```

```shell
wmic qfe Caption,Description,HotFixID,InstalledOn
```

List drives:
```shell
wmic logicaldisk get caption,description,providername
```

