---
title: Nextcloud direkt auf der StorageBox
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Nextcloud direkt auf der HostBrr StorageBox

HostBrr kombiniert die StorageBox mit DirectAdmin, PHP/LiteSpeed und Softaculous. Damit kann eine klassische PHP-Anwendung wie Nextcloud grundsätzlich direkt im Hosting-Account installiert werden, sofern die für die konkrete Nextcloud-Version notwendigen Anforderungen erfüllt sind.

## Architektur

```text
Browser / Apps
      ↓ HTTPS
HostBrr DirectAdmin/LiteSpeed
      ↓
Nextcloud + Datenbank + Storage
auf der StorageBox
```

## Vorteil

- kein zusätzlicher VPS nötig
- kurze Einrichtung über Softaculous möglich
- Speicher liegt direkt am Anwendungssystem
- kein zusätzlicher SFTP-Hop zwischen Nextcloud und Daten

## Nachteil

- Shared-Hosting-Ressourcenlimits
- keine Root-Rechte
- PHP-/Datenbank-Versionen werden vom Provider vorgegeben
- Background Jobs und lange Prozesse können stärker eingeschränkt sein
- HDD-Storage ist für datenbank-/metadatenintensive Anwendungen nicht ideal

## Vor der Installation prüfen

Nicht einfach auf „Installieren“ klicken. Zuerst dokumentieren:

```text
PHP-Version:
PHP memory_limit:
PHP upload_max_filesize:
PHP max_execution_time:
Datenbanktyp/-version:
Cron verfügbar:
HTTPS/Let's Encrypt:
Quota:
Softaculous Nextcloud-Version:
```

Offizielle Dokumentation:

- Nextcloud System Requirements: https://docs.nextcloud.com/server/stable/admin_manual/installation/system_requirements.html
- Nextcloud Installation: https://docs.nextcloud.com/server/stable/admin_manual/installation/index.html
- Nextcloud Background Jobs: https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/background_jobs_configuration.html
- Softaculous: https://www.softaculous.com/
- DirectAdmin: https://docs.directadmin.com/

## Domain

Empfohlen ist eine eigene Subdomain, beispielsweise:

```text
cloud.example.net
```

DNS auf HostBrr zeigen lassen, Domain in DirectAdmin anlegen und anschließend ein gültiges TLS-Zertifikat einrichten.

## Installation über Softaculous

Wenn Nextcloud in Softaculous angeboten wird:

1. DirectAdmin öffnen.
2. Softaculous starten.
3. Nextcloud auswählen.
4. Ziel-Domain festlegen.
5. Datenbank-/Admin-Daten sicher setzen.
6. Installation durchführen.
7. anschließend Nextcloud-Systemchecks prüfen.

Die tatsächlich angebotene Nextcloud-Version muss gegen die aktuelle unterstützte Version geprüft werden.

## Cron statt AJAX

Für eine produktive Nextcloud sollte nach Möglichkeit Cron für Background Jobs verwendet werden. Auf Shared Hosting muss geprüft werden, welche Cron-Frequenz HostBrr zulässt und welcher PHP-CLI-Pfad verwendet werden muss.

## Datenbank

Die Datenbank liegt in dieser Architektur ebenfalls im Shared-Hosting-Account. HostBrr weist bei der StorageBox grundsätzlich darauf hin, dass datenbankintensive Anwendungen nicht der ideale Workload für HDD-basierten Storage sind. Deshalb eignet sich diese Variante eher für private bzw. moderate Nutzung als für stark ausgelastete große Nextcloud-Instanzen.

## Backup

Mindestens sichern:

- Nextcloud-Konfiguration
- Datenverzeichnis
- Datenbank
- ggf. Apps/Themes

Ein Backup innerhalb derselben StorageBox ist keine vollständige Offsite-Strategie.

## Status

Die technische Möglichkeit ist plausibel und wurde in Community-Berichten über Softaculous/Nextcloud beschrieben. Welche aktuelle Nextcloud-Version auf unseren Boxen angeboten wird und welche PHP-/Cron-Limits gelten, wird später direkt verifiziert.
