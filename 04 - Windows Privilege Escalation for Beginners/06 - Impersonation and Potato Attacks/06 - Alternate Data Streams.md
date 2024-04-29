Alternate Data Streams - [https://blog.malwarebytes.com/101/2015/07/introduction-to-alternate-data-streams/](https://blog.malwarebytes.com/101/2015/07/introduction-to-alternate-data-streams/)

The root flag is hidden in an ADS of the file *hm.txt*

List all files incl. ADS:
```cmd
dir /R
```

![[Pasted image 20240417092957.png]]

Read the ADS with *more*:
```cmd
more < hm.txt:root.txt:$DATA
```
![[Pasted image 20240417093110.png]]