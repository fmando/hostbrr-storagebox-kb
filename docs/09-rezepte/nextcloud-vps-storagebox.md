---
title: Nextcloud auf VPS + HostBrr StorageBox
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Nextcloud auf VPS + HostBrr StorageBox

## Ziel

Nextcloud läuft kontrolliert auf einem eigenen VPS; die HostBrr StorageBox stellt zusätzlichen Massenspeicher über das offizielle SFTP-External-Storage-Backend bereit.

## Architektur

```text
Browser / Apps
      ↓ HTTPS
Nextcloud auf VPS
PHP + DB + Cron + Cache
      ↓ SFTP
HostBrr StorageBox
```

## Wann diese Architektur sinnvoll ist

Vorteile:

- volle Kontrolle über PHP, Datenbank und Background Jobs
- unabhängige Updates und Tuning
- StorageBox übernimmt primär Massenspeicher
- klare Trennung von Compute und günstigem HDD-Speicher

Nachteile:

- jeder Zugriff auf External Storage hängt von Netzwerk, SFTP und Latenz ab
- zusätzliche Fehlerdomäne zwischen VPS und StorageBox
- Metadaten-intensive Workloads können deutlich langsamer sein

Für häufig verwendete kleine Dateien ist lokaler VPS-Speicher meist attraktiver; Archive und große Dateien passen eher zum External-Storage-Modell.

## Voraussetzungen

- funktionierende Nextcloud auf dem VPS
- Background Jobs per Cron
- HostBrr-SFTP-Zugang
- dedizierter Zielordner
- Backupkonzept für VPS **und** StorageBox

Offizielle Dokumentation:

- https://docs.nextcloud.com/server/stable/admin_manual/configuration_files/external_storage_configuration_gui.html
- https://docs.nextcloud.com/server/stable/admin_manual/configuration_files/external_storage/sftp.html
- https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/background_jobs_configuration.html

## External Storage einrichten

1. App **External storage support** aktivieren.
2. Administration → External Storage öffnen.
3. Backend **SFTP** wählen.
4. HostBrr-Hostname und tatsächlichen SSH-Port eintragen.
5. Benutzer und Authentifizierung konfigurieren.
6. dedizierten Remote-Unterordner verwenden.
7. Verbindung in Nextcloud prüfen.

Beispielpfad nur als Schema:

```text
/home/USER/nextcloud-external
```

Die tatsächliche `$HOME`-Struktur der Box hat Vorrang.

## SSH-Key

Nextcloud unterstützt beim SFTP-Backend Public-Key-Authentifizierung. Der öffentliche Schlüssel wird auf dem Ziel autorisiert; private Schlüssel und Passphrasen gehören nicht in dieses Repository.

Siehe [SSH-Key-Härtung](../06-sicherheit/ssh-key-haertung.md).

## External Storage ist nicht das Nextcloud-Datadir

Diese Anleitung behandelt einen **externen Storage innerhalb von Nextcloud**. Das komplette primäre Nextcloud-Datadir auf einen SFTP/FUSE-Mount zu legen ist eine andere Architektur und wird hier nicht empfohlen.

## Dateierkennung und parallele Zugriffe

Dateien, die außerhalb von Nextcloud direkt auf der StorageBox geändert werden, werden nicht zwingend unmittelbar erkannt. Deshalb dieselben Daten möglichst nicht gleichzeitig per direktem SFTP und über Nextcloud bearbeiten.

Background Jobs und gegebenenfalls Dateiscans müssen bei solchen Workflows berücksichtigt werden.

## Automatisierung/Betrieb

Auf dem VPS:

- Cron für Nextcloud Background Jobs
- Monitoring von Nextcloud und Datenbank
- Monitoring des SFTP-Mounts/External-Storage-Status
- Warnung bei Quota-Problemen auf HostBrr

Die StorageBox sollte nicht als „immer verfügbar wie eine lokale Disk“ angenommen werden. Die Anwendung muss temporäre SFTP-/Netzwerkfehler verkraften.

## Backup

External Storage ist kein Backup. Separat sichern:

- Nextcloud-Konfiguration
- Datenbank
- Apps/Themes soweit erforderlich
- VPS-Konfiguration
- Daten auf der StorageBox

Wenn die StorageBox die Primärkopie einer Datei enthält, muss deren Backup auf **einem anderen Ziel** liegen.

Siehe [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md).

## Restore-Test

Ein vollständiger Test sollte mindestens umfassen:

1. Nextcloud-Datenbank und Konfiguration in Testumgebung wiederherstellen.
2. SFTP External Storage erneut anbinden.
3. Testdateien öffnen und herunterladen.
4. Berechtigungen und Shares prüfen.
5. Verhalten testen, wenn die StorageBox kurz nicht erreichbar ist.

## Typische Fehler

### External Storage bleibt rot/unerreichbar

Hostname, SSH-Port, Benutzer, Authentifizierung und Zielpfad prüfen. Verbindung zunächst außerhalb von Nextcloud per SFTP testen.

### Neue extern hochgeladene Dateien fehlen

Direkte Änderungen außerhalb Nextcloud vermeiden bzw. Scan-/Background-Mechanismen berücksichtigen.

### Viele kleine Dateien sind langsam

SFTP-Latenz und Metadatenzugriffe können dominieren. Siehe [Latenz & Routing](../07-performance/latenz-routing.md) und [Große vs. kleine Dateien](../07-performance/grosse-kleine-dateien.md).

## HostBrr-spezifisch noch zu verifizieren

- SFTP-Authentifizierung mit Nextcloud Public Key
- Timeouts und Verbindungsstabilität
- Performance großer Dateien
- Performance vieler kleiner Dateien
- Verhalten bei HostBrr-Wartung/kurzer Nichterreichbarkeit
- Unterschiede 2 TB vs. 8 TB
