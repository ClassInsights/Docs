# Erste Schritte

Hier finden Sie die Anleitung, um ClassInsights von Grund auf zu installieren!
Wir haben uns sehr bemüht die gesamte Installation sehr selbsterklärend und so einfach wie möglich zu gestalten.
Sollten Sie dennoch Schwierigkeiten haben können Sie sich jederzeit gerne [per Email](mailto:office@classinsights.at) bei uns melden!

?> **Alle Installationsskripte sind zu 100% Open-Source damit völlig nachvollziehbar ist, was beim Ausführen passiert!**

## Voraussetzungen

Neben einer **ClassInsights-Lizenz** benötigen Sie:

- **Private IP** des Servers, auf dem Dashboard und lokale API laufen sollen (Linux empfohlen; Windows-Support folgt bald)
- **Firewall Konfiguration** dass alle Schulcomputer den Server, der lokalen ClassInsights-API, auf den TCP Ports **52000** und **52001** erreichen können
- **Windows Active Directory** (diese Anleitung setzt eine Windows-AD-Umgebung voraus)

!> Wenn Ihre Schule ein anderes Authentifizierungs- oder Verzeichnisdienst-System nutzt, setzen Sie sich bitte vor Beginn mit uns in Verbindung: office@classinsights.at

---

## Vorbereitung auf dem Domain Controller

Öffnen Sie die PowerShell und befolgen Sie die Anleitung für diese Sektion idealerweise einem Ihrer Domain Controller.

**1. ExecutionPolicy ändern**
Bevor wir starten, vergewissern Sie sich, dass Sie PowerShell-Skripte ausführen dürfen:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

> Nun sollten lokale und signierte Skripte aus dem Internet erlaubt sein.

**2. Installationsskript herunterladen**

Nun laden wir uns die neuste Version des [ClassInsights Installers](https://github.com/ClassInsights/Installer) herunter und führen diesen aus:

```powershell
Invoke-WebRequest -Uri https://raw.githubusercontent.com/classinsights/installer/main/classinsights.ps1 -OutFile .\classinsights.ps1
```

**3. Installer starten**

```powershell
.\classinsights.ps1
```

> Der Assistent führt Sie nun durch die Erstellung aller benötigten Dateien.

**4. Lizenz-Key & Server-IP eingeben**

Bei dem "ClassInsights API Server" handelt es sich um den Server, auf dem Sie die lokale API und das Dashboard installieren werden.

```
Bitte gib deinen ClassInsights Lizenz Key ein
Lizenz: <Ihr-Lizenz-Key>

Was ist die IP Addresse des ClassInsights API Server?
IP: <Server-IP>    # z. B. 172.16.32.104
```

**5. Dateien auf den Server hochladen**

Wenn der Assistent fragt:

```
Wollen Sie die Dateien für die lokale API nun auf den Server hochladen? (J/N) J
```

geben Sie Ihre SSH-Zugangsdaten für Ihren lokalen ClassInsights Server ein, damit alle benötigten Dateien auf den Zielserver kopiert werden:

```
Username: <Ihr-Username>
IP des Servers: <Server-IP>
```

> Bei Problemen können Sie den Befehl auch manuell eingeben:
> scp -r api username@server:~/."

Nun können wir uns ein `ClassInsights` Gruppenrichtlinienobjekt erstellen lassen:

```
Wollen Sie nun zu der Erstellung des Gruppenrichtlinienobjekts übergehen? (J/N): J
```

> Hierbei wird das generierte `gpo_install.ps1` Skript ausgeführt

**Verzeichnisstruktur zur Orientierung**

Am Ende dieser Sektion sollte Ihr Verzeichnis so aussehen:

```
api/
├─ api.env
├─ cert.pfx
├─ classinsights.sh
├─ docker-compose.yml
gpo/
├─ gpo_install.ps1
├─ ClassInsights_CA.cer
├─ ClassInsights.msi
classinsights.ps1
```

## Konfigurierung der Gruppenrichtlinie (GPO)

Nun erstellen wir die GPO, dass alle Schulcomputer automatisch den ClassInsights WinService installieren.

**1. Dateien für Clients bereitstellen**
Kopieren Sie hierfür folgende Dateien in einen freigegebenen Netzwerk-Ordner
<br>(z. B. `\\ihre.ad.domain\SYSVOL\ihre.ad.domain\scripts`):

- `ClassInsights_CA.cer`
- `ClassInsights.msi`

> Die Dateien befinden sich in dem erstellten `gpo` Ordner

**2. GPO konfigurieren**
Als nächstes öffnen wir die Gruppenrichtlinienverwaltung und wählen bei den Gruppenrichtlinienobjekte `ClassInsights` zum Bearbeiten aus:

**2.1 Softwareinstallation**

Wir navigieren zu:

```
Computerkonfiguration
└─ Richtlinien
   └─ Softwareeinstellungen
      └─ Softwareinstallation
```

- → Rechtsklick → `Neu > Paket` → `ClassInsights.msi` → `OK`

**2.2 Stammzertifikat importieren**

```
Computerkonfiguration
└─ Richtlinien
   └─ Windows-Einstellungen
      └─ Sicherheitseinstellungen
         └─ Richtlinien für öffentliche Schlüssel
            └─ Vertrauenswürdige Stammzertifizierungsstellen
```

- Rechtsklick → **Importieren** → `ClassInsights_CA.cer`
- Folgen Sie dem Zertifikat-Import-Assistenten

**3. GPO aktivieren**

Weisen Sie nun die **ClassInsights**-GPO den gewünschten Organisationseinheiten zu.

> Nach der nächsten Gruppenrichtlinienauffrischung installieren sich die Clients automatisch.

## Installation auf dem ClassInsights API Server

Nun müssen wir noch die lokale API und das Dashboard installieren.

**1. SSH-Verbindung herstellen**  
Verwenden Sie hierfür z. B. [Putty](https://www.putty.org/), PowerShell oder ein anderes Terminal:

```
ssh <Ihr-Username>@<Server-IP>
```

**2. In das API-Verzeichnis wechseln & Installation starten**

```bash
cd api
chmod +x classinsights.sh && sudo ./classinsights.sh install
```

- Das Skript installiert [Docker](https://www.docker.com/) und startet alle benötigten Container automatisch.
- Standard-Port für die API: **52001**.
- Standard-Port für das Dashboard: **52000**

> **Hinweis:** Stellen Sie sicher, dass Port 52000 und 52001 in Ihrer Firewall geöffnet sind und alle Clients Zugriff haben.

## URLs und Lehrergruppen festlegen

Rufen Sie im Browser https://classinsights.at auf und melden Sie sich an. Danach wählen Sie bei Ihrer Schule das Einstellungssymbol und fügen Ihre lokalen URLs ein:

- **Lokales Dashboard:** `https://<DEINE-SERVER-IP>:52000`
- **Lokale API:** `https://<DEINE-SERVER-IP>:52001`

Außerdem können Sie hier festlegen, welche Azure Gruppen Lehrer sind und somit Zugriff auf das Dashboard bekommen sollen.

Mit einem Klick auf `Zum Dashboard` gelangen Sie zum lokalen Dashboard

## Räume konfigurieren

Nun müssen nur noch die WebUntis Räume irgendwie mit den Computer verknüpft werden. Hierfür gibt es [mehrere Möglichkeiten](room-assignment.md), suchen Sie sich die für Sie geeignetste Variante aus.

## Fragen/Probleme?

> Bitte senden Sie alle Fehlermeldungen und eine Beschreibung an: office@classinsights.at
