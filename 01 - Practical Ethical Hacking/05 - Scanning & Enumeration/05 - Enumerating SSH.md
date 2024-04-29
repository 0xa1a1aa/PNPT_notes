Try to connect to the the ssh service:
```bash
ssh 10.0.0.10
```
Output:
```bash
Unable to negotiate with 10.0.0.10 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1
```

Try again with the offered key exchange algorithms:
```bash
ssh 10.0.0.10 -oKexAlgorithms=+diffie-hellman-group1-sha1
```
Output:
```bash
Unable to negotiate with 10.0.0.10 port 22: no matching host key type found. Their offer: ssh-rsa,ssh-dss
```

Try again with the offered host key algorithms:
```bash
ssh 10.0.0.10 -oKexAlgorithms=+diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-dss
```
Output:
```bash
Unable to negotiate with 10.0.0.10 port 22: no matching cipher found. Their offer: aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,arcfour,aes192-cbc,aes256-cbc,rijndael128-cbc,rijndael192-cbc,rijndael256-cbc,rijndael-cbc@lysator.liu.se
```

Try again with the offered cipher algorithms:
```bash
ssh 10.0.0.10 -oKexAlgorithms=+diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-dss -c aes256-cbc
```
Now the connection works, but we don't have the credentials:
![[Pasted image 20240305203245.png]]

Sometimes a banner is exposed, giving us more information.