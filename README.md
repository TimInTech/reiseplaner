https://timintech.github.io/reiseplaner/

Kurzfassung: Mit **PSWindowsUpdate** holst du nur Updates für deine bestehende Version. Ein **Feature-Upgrade auf 23H2/24H2 auf nicht unterstützter Hardware** erzwingst du damit nicht. Dafür brauchst du das **ISO + die Registry-Bypässe**, die du schon gesetzt hast.
Trotzdem hier die sauberen, funktionierenden PowerShell-Befehle für alle verfügbaren Updates deiner aktuellen Installation – inkl. Treibern – und wie du danach korrekt auf ein neues Build upgradest.

1. PSWindowsUpdate korrekt einrichten und alles installieren
   Öffne PowerShell als Administrator und führe exakt Folgendes aus:

```powershell
# TLS & Paketquelle vorbereiten
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
Install-PackageProvider -Name NuGet -Force
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted

# Modul installieren & laden
Install-Module PSWindowsUpdate -Force
Import-Module PSWindowsUpdate

# Alle Updates (inkl. Microsoft Update) installieren und bei Bedarf neu starten
Get-WindowsUpdate -MicrosoftUpdate -AcceptAll -Install -AutoReboot

# Nur Treiber-Updates (optional)
Get-WindowsUpdate -Category 'Drivers' -AcceptAll -Install -AutoReboot
```

Hinweis zu deinem Fehler: `Install-WindowsUpdate -Driver ...` ist so falsch; benutze `Get-WindowsUpdate ... -Install …`. Der Parameter `-Driver` existiert so nicht—verwende die Kategorie „Drivers“ wie oben.

Windows-Update-Dienste zurücksetzen, falls etwas hängt:

```powershell
Stop-Service wuauserv -Force
Stop-Service bits -Force
Remove-Item "$env:windir\SoftwareDistribution" -Recurse -Force
Start-Service wuauserv
Start-Service bits
```

2. Feature-Upgrade auf 23H2/24H2 auf nicht unterstützter Hardware
   PSWindowsUpdate hilft hier nicht. Vorgehen stabil:

a) Bypass-Keys setzen/prüfen (hast du schon; zur Kontrolle):

```powershell
reg query "HKLM\SYSTEM\Setup\MoSetup" /v AllowUpgradesWithUnsupportedTPMOrCPU
reg query "HKLM\SYSTEM\Setup\LabConfig"
```

b) Offizielles Windows-11-ISO laden, per Doppelklick mounten (z. B. als Laufwerk `D:`) und **Setup direkt vom ISO** starten. So umgehst du den PC-Integritätscheck des Assistenten:

```powershell
D:\setup.exe
```

Wichtig: Wenn das Setup trotzdem meckert, starte es ohne dynamische Updates:

```powershell
D:\setup.exe /auto upgrade /dynamicupdate disable
```

Das nutzt die bereits gesetzten Bypass-Flags und installiert das neue Build auch auf deinem i5-2400.

3. Optional: Systemprüfung/Bereinigung (sinnvoll vor dem Upgrade)

```powershell
sfc /scannow
DISM /Online /Cleanup-Image /RestoreHealth
DISM /Online /Cleanup-Image /StartComponentCleanup
```

Fazit:
Ja, mit den obigen PSWindowsUpdate-Befehlen bekommst du alle **aktuellen Patches und Treiber**. Für das **neueste Windows-11-Feature-Build** auf deiner nicht unterstützten Hardware musst du das **ISO + Registry-Bypass** nutzen, nicht den Assistenten. Wenn du willst, erstelle ich dir eine kleine `.bat`, die die Bypässe setzt und das ISO-Setup automatisch mit `/dynamicupdate disable` startet.
