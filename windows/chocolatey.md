
[Chocolatey](https://chocolatey.org/) is a package manager for Windows. 

# Install

1. Use an adminstrative shell. Following commands uses Powershell.
2. Check that `Get-ExecutionPolicy` doesn't return `Restricted`.
	1. If it returns `Restricted`, then run `Set-ExecutionPolicy AllSigned` or `Set-ExecutionPolicy Bypass -Scope Process`.
3. Run the following command:
```PowerShell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
4. *Optional:* Install [Chocolatey GUI](https://community.chocolatey.org/packages/ChocolateyGUI)
```PowerShell
choco install chocolateygui
```

For more instructions see [install instructions](https://chocolatey.org/install)