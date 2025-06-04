# HCPSMW Asset Manager Service (Linux & Windows)

**Hey, das ist ein super praktischer Dienst! Er wurde entwickelt, um alle möglichen Hardware-Infos von deinem Linux- oder Windows-System zu sammeln und sie dann ordentlich in einer JSON-Datei abzulegen. Wir haben ihn in Go (Golang) geschrieben, was ihn schlank und effizient macht. Das Coole daran: Du kannst ihn ganz einfach kompilieren und als eine einzelne Datei nutzen. Keine extra Pakete oder so nötig – einfach runterladen und loslegen!**

## Funktionen

* **CPU-Informationen:** Name, Hersteller, wie viele Kerne und logische Prozessoren er hat und wie schnell er gerade läuft.
* **RAM-Modul-Details:** Wie viel Speicherplatz sie haben (z.B. 16 GB), welche Geschwindigkeit und welchen Typ (wie DDR4 oder DDR5), die Seriennummer, den Formfaktor, den Hersteller und die Teilenummer.
* **Festplatten-Informationen:** Modell, Hersteller, Seriennummer, Größe und was für eine Art Platte es ist.
* **Netzwerkadapter-Details:** Name, Hersteller, MAC-Adresse und wie schnell sie sind (in Mbit/s).
* **Motherboard-Informationen:** Hersteller, Produktname, Seriennummer und Version.
* **GPU-Informationen:** Name, Videospeicher (VRAM) in MB, Treiberversion und Videoprozessor.
* **JSON-Ausgabe:** Alle gesammelten Daten landen in einer übersichtlichen JSON-Datei. Das ist super, wenn du die Infos später weiterverarbeiten willst!
* **Dienst-Integration:** Sowohl auf Linux (als Systemd-Dienst) als auch auf Windows (als Windows-Dienst) kann er automatisch beim Systemstart laufen.

## Installation

### Für Linux-Systeme

**Der Dienst für Linux kommt als Debian-Paket (**`.deb`), was eine einfache Installation auf Debian-basierten Systemen (wie Ubuntu) ermöglicht.

#### Voraussetzungen auf dem Linux-System

**Damit der Dienst auch wirklich alle Hardware-Infos abrufen kann, braucht er ein paar Helfer-Tools. Aber keine Sorge: Die sind als Abhängigkeiten im **`.deb`-Paket hinterlegt und werden automatisch mitinstalliert:

* `dmidecode`: Für die detaillierten RAM- und Motherboard-Infos.
* `util-linux`: Da ist `lsblk` drin, für die Festplatten-Infos.
* `iproute2`: Enthält `ip a` für die Netzwerkadapter-Infos.
* `pciutils`: Hier steckt `lspci` drin, für die GPU-Infos.

#### So geht die Installation Schritt für Schritt (Linux)

1. **Hol dir das `.deb`-Paket:** Schau einfach auf der [Releases-Seite](https://github.com/HTSW-SMW/Public_AssetManager/releases) deines GitHub-Repos vorbei und lade die neueste `hardware-info-service_X.Y.Z_amd64.deb`-Datei runter.
2. **Installier das Paket:** Öffne ein Terminal auf deinem Linux-System und gib diesen Befehl ein (denk dran, `X.Y.Z` durch die aktuelle Versionsnummer zu ersetzen):
   ```
   sudo apt install ./hardware-info-service_X.Y.Z_amd64.deb
   ```apt` kümmert sich dann um alles: Die benötigten Abhängigkeiten werden installiert und der Dienst wird eingerichtet. Fertig!


   ```

#### Mal schauen, ob der Dienst läuft (Linux)

**Nach der Installation kannst du ganz fix prüfen, ob der Dienst auch wirklich aktiv ist:**

```
systemctl status hardware-info-service.service

```

**Die Logs des Dienstes kannst du dir so anschauen:**

```
journalctl -u hardware-info-service.service -f

```

### Für Windows-Systeme

**Für Windows gibt es eine ausführbare **`.exe`-Datei, die du direkt über die Kommandozeile steuern kannst, um sie zu installieren, zu starten oder zu stoppen.

#### So geht die Installation Schritt für Schritt (Windows)

1. **Hol dir die `.exe`-Datei:** Geh auf die [Releases-Seite](https://github.com/HTSW-SMW/Public_AssetManager/releases) deines GitHub-Repos und lade die `AssetManager_Service.exe`-Datei für Windows runter.
2. **Installiere den Dienst:** Öffne die **Eingabeaufforderung (CMD) oder PowerShell als Administrator****. Dann navigiere zu dem Ordner, in den du die **`.exe`-Datei heruntergeladen hast. Führe den Dienstbefehl mit dem `install`-Parameter aus:

   ```
   C:\Pfad\zu\deinem\Download\AssetManager_Service.exe --install

   ```

   **(Ersetze **`C:\Pfad\zu\deinem\Download\` durch den tatsächlichen Pfad zur Datei.)
3. **Starte den Dienst:** Nach der Installation kannst du den Dienst direkt starten:

   ```
   C:\Pfad\zu\deinem\Download\AssetManager_Service.exe --start

   ```

   **Alternativ kannst du auch in die Windows "Dienste"-Verwaltung (services.msc) gehen, dort nach "HCPSMW\_ASSETMGR\_Service" suchen und ihn starten.**

oder aber du führst folgendes aus:
  ```
  C:\Pfad\zu\deinem\Download\AssetManager_Service.exe --install --start
  ```

#### Mal schauen, ob der Dienst läuft (Windows)

**Du kannst den Status des Dienstes ebenfalls über die Eingabeaufforderung (als Administrator) prüfen:**

```
sc query HCPSMW_ASSETMGR_Service

```

**Oder in der Windows "Dienste"-Verwaltung.**

#### Dienst deinstallieren (Windows)

**Wenn du den Dienst wieder entfernen möchtest, kannst du das ebenfalls über die Kommandozeile tun:**

```
C:\Pfad\zu\deinem\Download\AssetManager_Service.exe --uninstall

```

#### Dienst manuell stoppen (Windows)

**Um den Dienst manuell zu stoppen:**

```
C:\Pfad\zu\deinem\Download\AssetManager_Service.exe --stop

```

## Verwendung

**Der Dienst sammelt die Hardware-Informationen automatisch, sobald er gestartet wird. Die Daten speichert er dann in der folgenden Datei:**

* **Linux:**
  ```
  /var/log/hcpsmw/hardware_info.json

  ```
* **Windows:**
  ```
  C:\Programm Files(x86)\HCPSMW ASSTMGR\Data\Hardware.json

  ```

**Der Dienst ist so konfiguriert, dass er die Infos regelmäßig aktualisiert – standardmäßig alle 60 Sekunden. Praktisch, oder?**

## Geplante Features

**Wir arbeiten kontinuierlich daran, den HCPSMW Asset Manager Service noch besser zu machen! Hier sind ein paar Ideen für zukünftige Features:**

* **Grafische Benutzeroberfläche (GUI):** Eine Desktop-App, die dir alle gesammelten Hardware-Infos super übersichtlich anzeigt und mit der du ganz einfach mit dem Dienst interagieren kannst.
* **Web-Management-Interface:** Eine Oberfläche im Browser, über die du die Hardware-Infos auch aus der Ferne verwalten und ansehen kannst.
* **QR-Code-Generierung:** Stell dir vor, du könntest wichtige Hardware-Infos (wie Seriennummern) direkt als QR-Codes generieren! Das würde die Inventarisierung total vereinfachen.
* **Datenbank-Anbindung:** Die Hardware-Infos in einer Datenbank speichern, um Verläufe zu sehen, erweiterte Suchen zu machen und Berichte zu erstellen.
* **Benachrichtigungen:** Wenn sich Hardware ändert oder etwas Kritisches passiert, bekommst du eine Info!

## Mach mit!

**Hast du Ideen, Fehler gefunden oder möchtest du neue Funktionen beisteuern? Dann melde dich gerne! Erstell einfach ein Issue oder schick uns einen Pull Request. Wir freuen uns drauf!**
