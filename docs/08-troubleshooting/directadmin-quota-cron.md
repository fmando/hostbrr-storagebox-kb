---
title: DirectAdmin, Quota & Cron Troubleshooting
category: troubleshooting
status: maintained
last_reviewed: 2026-08-24
---
# DirectAdmin, Quota & Cron Troubleshooting

## DirectAdmin-Funktion fehlt

DirectAdmin-Funktionen können paket- oder benutzerabhängig freigeschaltet sein. Dazu gehören unter anderem SSH, Cron, PHP und SSL. Fehlt ein Menüpunkt, deshalb zuerst prüfen, ob die Funktion für den HostBrr-Account überhaupt vorgesehen ist.

## Quota stimmt scheinbar nicht

Drei Werte auseinanderhalten:

```bash
du -sh "$HOME"
du -sh "$HOME"/* 2>/dev/null | sort -h
df -h
```

`du` misst sichtbare Dateien in Verzeichnissen, `df` das Dateisystem. DirectAdmin kann zusätzlich systemseitige Quotas verwenden. Abweichungen bedeuten daher nicht automatisch Datenverlust.

Als Shared-Hosting-Nutzer können wir serverseitige Quota-Reparaturen aus der DirectAdmin-Admin-Dokumentation normalerweise nicht selbst durchführen. Bei dauerhaft falscher Anzeige: Messergebnisse dokumentieren und HostBrr-Support einschalten.

## Cronjob läuft manuell, aber nicht automatisch

Das ist ein sehr typisches Fehlerbild. Cron besitzt oft eine andere Umgebung als die interaktive Shell.

Schlecht:

```bash
rclone sync /data hostbrr:backup
```

Robuster:

```bash
/usr/bin/rclone sync /home/<USER>/data hostbrr:backup >> /home/<USER>/logs/rclone.log 2>&1
```

Prüfen:

```bash
which rclone
which php
which python3
env
```

Absolute Pfade für Programme, Skripte und Dateien verwenden.

## Cron-Output nicht wegwerfen

Während der Fehlersuche Output protokollieren oder die DirectAdmin-E-Mail-Ausgabe aktiv lassen. DirectAdmin empfiehlt bei Cron-Diagnosen ebenfalls, Ausgaben sichtbar zu halten.

## Doppelstarts vermeiden

Lange Backups dürfen nicht parallel übereinander laufen. Falls verfügbar:

```bash
flock -n /home/<USER>/tmp/backup.lock /home/<USER>/bin/backup.sh
```

## Ressourcenlimits

DirectAdmin unterstützt grundsätzlich benutzerspezifische Limits für CPU, RAM, I/O, IOPS und Tasks; diese können auch Cronjobs und Benutzerprozesse betreffen. Welche Limits HostBrr auf einer konkreten StorageBox setzt, muss separat dokumentiert werden.

## Weiterführende Dokumentation

- [DirectAdmin – Configuration files / User package features](https://docs.directadmin.com/developer/config_files/)
- [DirectAdmin – Filesystem and quotas](https://docs.directadmin.com/operation-system-level/os-general/filesystems-and-quotas.html)
- [DirectAdmin – Resource throttling](https://docs.directadmin.com/other-hosting-services/pro-resource-throttling/)
