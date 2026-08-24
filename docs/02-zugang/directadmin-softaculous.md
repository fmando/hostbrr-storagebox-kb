---
title: "DirectAdmin: Softaculous"
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---
# DirectAdmin: Softaculous

HostBrr nennt Softaculous als Bestandteil der StorageBox. Damit lassen sich unterstützte Webanwendungen über DirectAdmin installieren. Für unsere KB ist besonders Nextcloud interessant.

## Was Softaculous macht

Softaculous automatisiert typische Installationsschritte wie Dateien bereitstellen, Datenbank vorbereiten und Anwendungskonfiguration erzeugen.

Offizielle Website: https://www.softaculous.com/

Dokumentation: https://www.softaculous.com/docs/

## Automatische Installation ersetzt keine Architekturentscheidung

Nur weil eine Anwendung mit wenigen Klicks installierbar ist, heißt das nicht automatisch, dass die StorageBox für jeden Workload die beste Plattform ist.

Vor einer Installation prüfen wir:

- PHP-Anforderungen
- Datenbankanforderungen
- Speicherbedarf
- Hintergrundjobs
- Updateverfahren
- Backupverfahren
- Ressourcenlimits

## Nextcloud

HostBrr nennt Nextcloud als möglichen Anwendungsfall. Für eine dauerhafte Installation müssen insbesondere Background Jobs korrekt eingerichtet werden. Nextcloud empfiehlt für reguläre Serverinstallationen Cron gegenüber AJAX/Webcron.

Nextcloud Background Jobs: https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/background_jobs_configuration.html

## Installationsdaten dokumentieren

Für jede Softaculous-Installation sollten wir festhalten:

```text
Anwendung:
Version:
Domain/Subdomain:
Installationspfad:
Datenbank:
PHP-Version:
Cronjobs:
Backupstrategie:
Installationsdatum:
```

Keine Passwörter oder Secrets in die KB übernehmen.

## Updates

Automatische Updates sind bequem, können aber bei größeren Versionssprüngen problematisch sein. Vor Updates sollte ein wiederherstellbares Backup existieren.

## Deinstallation

Bei einer Deinstallation prüfen, ob Softaculous zusätzlich Datenbank und Datenverzeichnis entfernt. Vorher Backup erstellen und die ausgewählten Löschoptionen lesen.

## HostBrr-Checkliste

- Softaculous vorhanden?
- welche Anwendungen sichtbar?
- Nextcloud-Version
- automatische Updates möglich?
- automatische Backups möglich?
- Backupziel und Speicherverbrauch
- Cron-Integration
- PHP-Version auswählbar?

## Weiterführende Dokumentation

- Softaculous: https://www.softaculous.com/
- Softaculous Docs: https://www.softaculous.com/docs/
- Nextcloud Admin Manual: https://docs.nextcloud.com/server/latest/admin_manual/
- Nextcloud Background Jobs: https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/background_jobs_configuration.html
