---
title: Nextcloud
category: anwendungen
status: community-reported
last_reviewed: 2026-08-24
---

# Nextcloud mit der HostBrr StorageBox

[Nextcloud](https://nextcloud.com/) ist eine selbst hostbare Plattform für Dateien, Synchronisation und Zusammenarbeit. HostBrr nennt Nextcloud als Anwendung, die über die Shared-Hosting-/Softaculous-Umgebung der StorageBox eingesetzt werden kann.

Für die StorageBox sind zwei grundsätzlich unterschiedliche Architekturen interessant.

## Variante A: Nextcloud direkt auf der StorageBox

```text
Browser / Nextcloud Clients
          |
          v
HostBrr StorageBox
├── Webserver/PHP
├── Nextcloud
├── Datenbank
└── Nutzdaten
```

Vorteile sind die einfache Architektur und dass kein zusätzlicher VPS erforderlich ist. Zu beachten sind jedoch die Ressourcenlimits der Shared-Hosting-Umgebung und die HDD-orientierte Storage-Plattform. Nextcloud erzeugt neben großen Nutzdaten auch zahlreiche Metadaten- und Datenbankzugriffe.

Diese Variante sollten wir deshalb insbesondere mit größeren Dateibeständen erst nach einem Praxistest als Empfehlung kennzeichnen.

## Variante B: Nextcloud auf einem VPS, StorageBox für Daten/Backups

```text
VPS
├── Nextcloud
├── PHP
├── Datenbank
└── optional Redis
       |
       v
HostBrr StorageBox
└── Storage / Backup
```

Diese Trennung gibt der Anwendung und Datenbank besser kontrollierbare CPU-, RAM- und I/O-Ressourcen. Allerdings muss genau entschieden werden, **wie** die StorageBox angebunden wird. Ein beliebig gemountetes SFTP-Dateisystem als primäres Nextcloud-Datenverzeichnis ist nicht automatisch eine gute Architektur.

Für externe Speicher besitzt Nextcloud eine eigene External-Storage-Funktion. Welche Backend-Variante für HostBrr sinnvoll ist, wird separat untersucht.

## Installation

Wenn Softaculous im StorageBox-Account Nextcloud anbietet, kann die Anwendung darüber installiert werden. Für eine dauerhafte KB-Dokumentation sollten wir dennoch die offiziellen Nextcloud-Anforderungen als Referenz verwenden, weil Softaculous lediglich den Installationsprozess automatisiert.

## Vor produktiver Nutzung prüfen

- von HostBrr bereitgestellte PHP-Version und Extensions
- PHP Memory Limit
- Datenbanktyp und Limits
- Cron-Unterstützung
- Nextcloud Background Jobs
- maximale Uploadgröße
- verfügbare IOPS/I/O-Limits
- Update-Verhalten von Softaculous
- Backup von Daten **und** Datenbank

## Weiterführende Dokumentation

- [Nextcloud – offizielle Projektseite](https://nextcloud.com/)
- [Nextcloud Admin Manual](https://docs.nextcloud.com/server/latest/admin_manual/)
- [Nextcloud – Installation](https://docs.nextcloud.com/server/latest/admin_manual/installation/)
- [Nextcloud – System requirements](https://docs.nextcloud.com/server/latest/admin_manual/installation/system_requirements.html)
- [Nextcloud – Background jobs](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/background_jobs_configuration.html)
- [Nextcloud – External Storage](https://docs.nextcloud.com/server/latest/admin_manual/configuration_files/external_storage_configuration_gui.html)
- [Softaculous](https://www.softaculous.com/)

## Bewertung

Direkte Installation ist technisch interessant und wird von HostBrr beworben. Für größere oder wichtige Nextcloud-Instanzen sollte die Entscheidung aber nicht allein auf „Softaculous kann es installieren“ beruhen. Performance, Background Jobs, Datenbank und Restore müssen zusammen betrachtet werden.
