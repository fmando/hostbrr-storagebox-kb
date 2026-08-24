---
title: Nextcloud Troubleshooting
category: troubleshooting
status: maintained
last_reviewed: 2026-08-24
---
# Nextcloud Troubleshooting auf/mit der StorageBox

Zuerst unterscheiden, welche Architektur verwendet wird:

1. Nextcloud läuft direkt im HostBrr-Webhosting, oder
2. Nextcloud läuft auf einem eigenen VPS und HostBrr dient nur als Storage-/Backupziel.

Die Diagnose unterscheidet sich erheblich.

## Nextcloud direkt auf HostBrr

Typische Prüfpunkte:

- unterstützte PHP-Version
- benötigte PHP-Extensions
- PHP Memory Limit
- Datenbank erreichbar
- Cron/Background Jobs
- Dateirechte
- Quota
- Webserver-/PHP-Limits

Nextcloud besitzt im Adminbereich eine Systemübersicht mit Warnungen. Diese Meldungen zuerst dokumentieren, bevor Konfiguration geändert wird.

## Background Jobs

Für produktive Installationen empfiehlt Nextcloud Cron gegenüber AJAX. Auf Shared Hosting hängt die Umsetzung davon ab, ob HostBrr Cron für den Account freigeschaltet hat.

## `occ`

Wenn CLI-PHP und `occ` verfügbar sind, ist es eines der wichtigsten Diagnosewerkzeuge:

```bash
php occ status
php occ config:list system
```

Bei mehreren PHP-Versionen kann der korrekte CLI-PHP-Pfad entscheidend sein.

## Datenverzeichnis auf Remote-Mount

Wenn Nextcloud auf einem VPS läuft und die StorageBox per SSHFS/rclone als primäres Datenverzeichnis eingebunden wird, entsteht eine zusätzliche Fehler- und Latenzschicht. Vor einer solchen Architektur sollten direkte Storage- und Mount-Tests erfolgen.

Für viele Setups ist es einfacher, Nextcloud lokal/VPS-seitig zu betreiben und HostBrr als verschlüsseltes Backupziel zu verwenden.

## Backup vollständig denken

Ein Nextcloud-Backup umfasst nicht nur Nutzdateien. Konfiguration, Datenverzeichnis und Datenbank gehören zusammen.

## Weiterführende Dokumentation

- [Nextcloud Admin Manual](https://docs.nextcloud.com/server/latest/admin_manual/)
- [Nextcloud System Requirements](https://docs.nextcloud.com/server/latest/admin_manual/installation/system_requirements.html)
- [Nextcloud Background Jobs](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/background_jobs_configuration.html)
- [Nextcloud Backup](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/backup.html)
