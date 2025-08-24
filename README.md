https://timintech.github.io/reiseplaner/




```bat
reg query "HKLM\SYSTEM\Setup\MoSetup" /v AllowUpgradesWithUnsupportedTPMOrCPU
```

```bat
reg query "HKLM\SYSTEM\Setup\LabConfig"
```




reg query "HKLM\SYSTEM\Setup\MoSetup" /v AllowUpgradesWithUnsupportedTPMOrCPU
reg query "HKLM\SYSTEM\Setup\LabConfig"





reg add "HKLM\SYSTEM\Setup\LabConfig" /v BypassTPMCheck /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\Setup\LabConfig" /v BypassCPUCheck /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\Setup\LabConfig" /v BypassSecureBootCheck /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\Setup\LabConfig" /v BypassRAMCheck /t REG_DWORD /d 1 /f


---

# 🪟 Windows 11 – Einrichtung & Reparatur

**Ziel:**  
Nach einer Neuinstallation von Windows 11 das System **vollständig aktualisieren**, **alle Treiber installieren**, **Skriptausführung aktivieren** und bei Bedarf **reparieren & bereinigen**.

---

## 🛠️ 1. Vorbereitung & PowerShell-Setup

### PowerShell als Administrator starten
- `Win + X` → **Windows PowerShell (Admin)**
- Oder:
```powershell
Start-Process powershell -Verb runAs
````

### Skriptausführung erlauben (Execution Policy)

```powershell
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```

> Erlaubt lokale Skripte ohne Signatur, Internet-Skripte nur mit gültiger Signatur.

---

## 🔄 2. Windows Updates & Treiber installieren

### PSWindowsUpdate-Modul installieren

```powershell
Install-Module PSWindowsUpdate -Force -SkipPublisherCheck
Import-Module PSWindowsUpdate
```

### Alle Windows-Updates + Treiber installieren

```powershell
# Windows-Updates (inkl. Treiber) vollständig einspielen
Get-WindowsUpdate -AcceptAll -Install -AutoReboot

# Nur Treiber-Updates separat
Install-WindowsUpdate -Driver -AcceptAll -Install -AutoReboot
```

### Update-Dienst bei Fehlern zurücksetzen

```powershell
Stop-Service wuauserv, bits -Force
Remove-Item "$env:windir\SoftwareDistribution" -Recurse -Force
Start-Service wuauserv, bits
```

---

## 💻 3. Zusätzliche Software installieren (Winget)

### Winget-Quellen aktualisieren

```powershell
winget source update
```

### Wichtige Tools installieren

```powershell
winget install --id=Google.Chrome -e
winget install --id=7zip.7zip -e
winget install --id=Microsoft.VisualStudioCode -e
```

---

## 🧰 4. Systemprüfung & Reparatur

### Integritätsprüfung – beschädigte Systemdateien reparieren

```powershell
sfc /scannow
```

### Windows-Komponenten reparieren

```powershell
DISM /Online /Cleanup-Image /RestoreHealth
```

### Komponentenspeicher bereinigen

```powershell
DISM /Online /Cleanup-Image /StartComponentCleanup
```

---

## 🧹 5. Bereinigung & Optimierung

### Temporäre Dateien löschen

```powershell
del /s /q C:\Windows\Temp\*
del /s /q C:\Users\%username%\AppData\Local\Temp\*
```

### DNS-Cache leeren

```powershell
ipconfig /flushdns
```

### Autostart prüfen

```powershell
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command
```

---

## 🚨 6. Notfallmaßnahmen

### Wiederherstellungspunkt erstellen

```powershell
Checkpoint-Computer -Description "Frisch eingerichtet" -RestorePointType MODIFY_SETTINGS
```

### Inplace-Upgrade vorbereiten (Neuinstallation ohne Datenverlust)

```powershell
Reg.exe Add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion" /v AllowInplaceUpgrade /t REG_DWORD /f /d 1
```
