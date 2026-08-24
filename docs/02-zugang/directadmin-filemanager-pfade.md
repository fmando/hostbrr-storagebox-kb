---
title: "DirectAdmin: File Manager & Verzeichnisstruktur"
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---
# DirectAdmin: File Manager & Verzeichnisstruktur

Dieses Kapitel beschreibt, wie Dateien in DirectAdmin verwaltet werden und welche Pfade für StorageBox-Howtos relevant sind. Da HostBrr die konkrete Account-Struktur beeinflussen kann, müssen reale Pfade auf der jeweiligen StorageBox geprüft werden.

## Warum die Pfade wichtig sind

Pfade tauchen später bei rsync, rclone, Cronjobs, Nextcloud, Webhosting und Restore-Vorgängen auf. Ein falscher Zielpfad kann dazu führen, dass Daten im Webroot landen oder ein Backup nicht dort gespeichert wird, wo man es erwartet.

## File Manager

DirectAdmin stellt einen webbasierten File Manager bereit. Er eignet sich für einzelne Dateien, Rechtekontrolle und schnelle Prüfungen. Für große Datenmengen sind SFTP, rsync oder rclone meist geeigneter.

Offizielle Dokumentation: https://docs.directadmin.com/directadmin/general-usage/file-manager.html

## Typische Struktur

DirectAdmin-Installationen verwenden häufig Strukturen nach diesem Muster:

```text
/home/USERNAME/
├── domains/
│   └── example.org/
│       ├── public_html/
│       ├── private_html/
│       └── logs/
├── backups/
└── ...
```

Wichtig: Dieses Schema ist eine Orientierung, keine ungeprüfte HostBrr-Garantie.

## Webroot vs. Backupdaten

`public_html` ist typischerweise öffentlich über den Webserver erreichbar. Sensible Backups gehören daher grundsätzlich nicht in einen öffentlich erreichbaren Webroot.

Empfehlung für unsere KB: Backupziele immer außerhalb von `public_html` anlegen, sofern der Account dies zulässt.

## Pfad selbst feststellen

Nach SSH-Login:

```bash
pwd
printf '%s\n' "$HOME"
ls -la
find "$HOME" -maxdepth 2 -type d | sort | head -100
```

Damit lässt sich die reale Struktur dokumentieren, ohne Annahmen aus generischen DirectAdmin-Installationen zu übernehmen.

## Dateirechte

Zur Diagnose:

```bash
ls -lah
stat DATEI
```

Rechte sollten nicht pauschal mit `chmod -R 777` repariert werden. Das verschlechtert die Sicherheit und kaschiert häufig die eigentliche Ursache.

## Speicherverbrauch

Nützliche Shell-Befehle:

```bash
du -sh "$HOME"
du -h --max-depth=1 "$HOME" | sort -h
df -h
```

Die in DirectAdmin angezeigte Quota kann von `df` abweichen, weil Shared-Hosting-Quotas accountbezogen umgesetzt werden können.

## HostBrr-Checkliste

Auf einer realen StorageBox dokumentieren:

- `$HOME`
- Verzeichnisnamen direkt unter `$HOME`
- tatsächliche Domainstruktur
- Webroot
- geeigneter privater Backup-Pfad
- File-Manager-Funktionen
- maximale Uploadgröße im Browser
- Quota-Anzeige
- Symlink-Verhalten

## Weiterführende Dokumentation

- DirectAdmin Documentation: https://docs.directadmin.com/
- DirectAdmin File Manager: https://docs.directadmin.com/directadmin/general-usage/file-manager.html
