# Method 1

Sysinternals tool to see all autorun programs:
```cmd
C:\Users\User\Desktop\Tools\Autoruns\Autoruns64.exe
```

Search files we can write to:
```cmd
C:\Users\User\Desktop\Tools\Accesschk\accesschk64.exe -wvu "C:\Program files\Autorun Program"
```
Args:
`-w`: Show files with write permissions
`-v`: Verbose
`-u`: Suppress errors

# Method 2

Install the [**powerUp.ps1**](https://github.com/PowerShellMafia/PowerSploit) tool in your system
- Go to powerup tool and open cmd There by cicking right shift + right click
- type `powershell -ep bypass` (-ep = execution bypass policy)
- type `. .\powerUp.ps1`
- type `Invoke-AllChecks`