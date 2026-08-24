---
title: StorageBox als Cloud-Laufwerk mit Cache
category: rezepte
status: community-reported
last_reviewed: 2026-08-24
---
# StorageBox als Cloud-Laufwerk mit lokalem Cache

## Ziel

Großer günstiger Remote-Speicher der HostBrr StorageBox wird mit lokalem SSD/NVMe-Cache auf einem VPS oder Linux-System kombiniert.

## Variante A – rclone mount

Offizielle Dokumentation:

- [rclone mount](https://rclone.org/commands/rclone_mount/)
- [rclone VFS](https://rclone.org/commands/rclone_mount/#vfs-file-caching)
- [rclone SFTP](https://rclone.org/sftp/)

Beispiel:

```bash
mkdir -p /mnt/hostbrr /var/cache/rclone-hostbrr

rclone mount hostbrr: /mnt/hostbrr \
  --vfs-cache-mode full \
  --cache-dir /var/cache/rclone-hostbrr \
  --vfs-cache-max-size 50G \
  --vfs-cache-max-age 24h
```

Die Cachegröße muss zum lokalen freien Speicher passen.

## Variante B – JuiceFS

Community-Berichte beschreiben HostBrr StorageBox + SFTP + JuiceFS mit lokalem NVMe-Cache als interessante Lösung bei Workloads mit vielen Metadatenoperationen.

Offizielle Dokumentation:

- [JuiceFS Documentation](https://juicefs.com/docs/)
- [JuiceFS cache](https://juicefs.com/docs/community/cache_management/)

Das ist eine fortgeschrittene Architektur und wird in der KB derzeit als `community-reported` behandelt, bis wir sie selbst reproduziert haben.

## Wann ist das sinnvoll?

Gut geeignet für:

- große Medienarchive
- selten geänderte Daten
- Cloud-Drive-artigen Zugriff
- Anwendungen, deren Hot-Set in einen lokalen Cache passt

Weniger geeignet für:

- latenzkritische Datenbanken
- stark transaktionale Anwendungen
- Workloads, die echte lokale POSIX-Semantik voraussetzen

## HostBrr-Erfahrungen aus der Community

Es existieren Berichte über rclone/SFTP-Mounts mit VFS-Cache. Bei vielen kleinen Dateien bzw. Metadatenzugriffen wurde JuiceFS als schneller empfunden. Ein Nutzer beschreibt eine etwa 350-GB-Immich-Datenablage hinter einem 50-GB-NVMe-Cache.

Diese Werte sind Erfahrungsberichte, keine garantierten HostBrr-Produkteigenschaften.

## Sicherheitsfrage

Ein Cache ersetzt kein Backup. Ebenso macht ein Mount die Remote-Daten für Anwendungen direkt erreichbar; kompromittierte Anwendungen können daher je nach Berechtigungen auch Remote-Dateien verändern oder löschen.

Für unveränderliche/offline Backupgenerationen ist ein separates Verfahren nötig.
