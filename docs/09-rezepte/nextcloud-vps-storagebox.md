---
title: Nextcloud auf VPS + HostBrr StorageBox
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Nextcloud auf VPS + HostBrr StorageBox

Diese Architektur trennt Rechenleistung und Massenspeicher:

```text
Browser / Apps
      ↓ HTTPS
Nextcloud auf VPS
      ↓ SFTP
HostBrr StorageBox
```

Die Nextcloud-Anwendung, PHP, Webserver und Datenbank laufen auf einem eigenen VPS. Die HostBrr-StorageBox wird über Nextcloud **External Storage** als SFTP-Speicher eingebunden.

## Warum diese Architektur?

Vorteile gegenüber einer vollständigen Nextcloud-Installation auf Shared Hosting:

- volle Kontrolle über PHP und Datenbank
- eigene Cron-/Background-Job-Konfiguration
- leichteres Tuning
- unabhängige Updates
- StorageBox übernimmt primär Massenspeicher

Nachteile:

- jeder Dateizugriff auf externen Storage hängt von Netzwerk, SFTP und Latenz ab
- zusätzliche Fehlerquelle zwischen VPS und StorageBox
- Metadaten-intensive Workloads können langsamer sein

## Nextcloud External Storage

Nextcloud unterstützt SFTP offiziell als External-Storage-Backend. Unterstützt werden Passwort- und Public-Key-Authentifizierung.

Offizielle Dokumentation:

- External Storage: https://docs.nextcloud.com/server/stable/admin_manual/configuration_files/external_storage_configuration_gui.html
- SFTP Backend: https://docs.nextcloud.com/server/stable/admin_manual/configuration_files/external_storage/sftp.html

## Einrichtung

1. Nextcloud-App **External storage support** aktivieren.
2. Administration → External Storage öffnen.
3. Backend **SFTP** auswählen.
4. HostBrr-Hostname eintragen.
5. tatsächlichen SSH-Port eintragen.
6. Benutzer und Authentifizierung konfigurieren.
7. einen dedizierten Remote-Unterordner verwenden.

Beispiel:

```text
/home/USER/nextcloud-external
```

## SSH-Key

Für Public-Key-Authentifizierung kann Nextcloud ein Schlüsselpaar erzeugen. Der öffentliche Schlüssel muss anschließend auf dem SFTP-Ziel autorisiert werden.

Private Keys gehören nicht in dieses Git-Repository.

## Wichtig: External Storage ist nicht das gleiche wie Nextcloud-Datadir

Diese Anleitung behandelt bewusst einen **externen Storage-Mount innerhalb von Nextcloud**. Das komplette primäre Nextcloud-Datadir einfach auf einen FUSE-/SFTP-Mount zu legen, ist eine andere Architektur mit anderen Anforderungen und sollte nicht ohne Tests empfohlen werden.

## Dateierkennung

Dateien, die außerhalb von Nextcloud direkt auf der StorageBox verändert werden, erscheinen unter Umständen nicht sofort in Nextcloud. Nextcloud dokumentiert dafür Scan-/Background-Mechanismen.

Deshalb möglichst vermeiden, dass Benutzer dieselben Dateien gleichzeitig direkt per SFTP und über Nextcloud bearbeiten.

## Backup bleibt notwendig

External Storage ersetzt kein Backup. Mindestens separat sichern:

- Nextcloud-Konfiguration
- Datenbank
- VPS-System-/Anwendungskonfiguration
- StorageBox-Daten

Die StorageBox sollte also nicht gleichzeitig einzige Primärkopie und einzige Backupkopie derselben Daten sein.

## Performance

Diese Architektur ist besonders interessant für große Dateien und Archivdaten. Bei vielen kleinen Dateien und häufigen Metadatenoperationen erwarten wir stärkere Auswirkungen der SFTP-Latenz. Das wird später Teil der Praxisvergleiche.
