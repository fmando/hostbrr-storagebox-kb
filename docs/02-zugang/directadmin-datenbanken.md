---
title: "DirectAdmin: Datenbanken"
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---
# DirectAdmin: Datenbanken

Die HostBrr StorageBox ist mehr als reiner SFTP-Speicher: je nach Paket stehen auch Datenbanken zur Verfügung. Das ist insbesondere für Anwendungen wie Nextcloud relevant.

## Grundprinzip

Datenbanken werden im DirectAdmin-Benutzerbereich angelegt. Typischerweise werden Datenbankname, Datenbankbenutzer und Passwort getrennt verwaltet.

Offizielle DirectAdmin-Dokumentation: https://docs.directadmin.com/

## Keine Zugangsdaten ins Repository

Folgende Werte gehören niemals in Git:

```text
DB_PASSWORD
DATABASE_URL
config.php mit echten Credentials
.env mit Secrets
```

Für Beispiele verwenden wir Platzhalter.

## Anwendungskonfiguration

Beispielhaft:

```text
DB_HOST=localhost
DB_NAME=USERNAME_app
DB_USER=USERNAME_app
DB_PASSWORD=GEHEIM
```

Die echten Werte müssen aus DirectAdmin bzw. der HostBrr-Konfiguration übernommen werden.

## Backup einer Datenbank

Dateibackup und Datenbankbackup sind zwei verschiedene Dinge. Bei einer Webanwendung reicht es nicht, nur `public_html` oder das Datenverzeichnis zu kopieren.

Falls `mysqldump` im SSH-Account verfügbar ist:

```bash
mysqldump -h localhost -u DBUSER -p DBNAME > database.sql
```

Das Passwort sollte nicht direkt in der Shell-History stehen.

Komprimiert:

```bash
mysqldump -h localhost -u DBUSER -p DBNAME | gzip > database.sql.gz
```

Ob `mysqldump` und welche Datenbankengine auf der aktuellen HostBrr StorageBox verfügbar sind, muss verifiziert werden.

## Restore

Ein Restore sollte separat dokumentiert und getestet werden. Beispiel für MySQL/MariaDB-kompatible Systeme:

```bash
gunzip -c database.sql.gz | mysql -h localhost -u DBUSER -p DBNAME
```

Vor produktiven Restores immer prüfen, ob vorhandene Daten überschrieben werden.

## Nextcloud

Für Nextcloud sind Datenbank und Dateidaten gemeinsam zu betrachten. Ein konsistentes Backup benötigt zusätzlich die passende Nextcloud-Backupstrategie und ggf. Maintenance Mode.

Offizielle Nextcloud-Dokumentation: https://docs.nextcloud.com/server/latest/admin_manual/maintenance/backup.html

## HostBrr-Checkliste

Zu prüfen:

- angebotene Datenbankengine
- Version
- Anzahl erlaubter Datenbanken
- maximale Datenbankgröße
- Datenbank-Host
- phpMyAdmin oder vergleichbares Tool
- `mysql`/`mariadb` CLI verfügbar?
- `mysqldump` verfügbar?
- Remote-Zugriff erlaubt oder nur localhost?

## Weiterführende Dokumentation

- DirectAdmin Documentation: https://docs.directadmin.com/
- Nextcloud Backup: https://docs.nextcloud.com/server/latest/admin_manual/maintenance/backup.html
