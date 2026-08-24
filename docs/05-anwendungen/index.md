---
title: Anwendungen
category: anwendungen
status: maintained
last_reviewed: 2026-08-24
---
# Anwendungen

Die StorageBox ist eine Kombination aus großem Shared-Storage und DirectAdmin-Webhosting. Dadurch sind mehr Anwendungen möglich als bei einem reinen SFTP-Speicher – aber deutlich weniger als auf einem VPS mit Root-Zugriff.

## Leitfrage

Entscheidend ist nicht nur, ob sich Software irgendwie starten lässt, sondern ob sie zum Betriebsmodell passt.

**Gut passend:**

- Backups und Archive
- SFTP/rclone-Dateispeicher
- statische Websites und Downloads
- einfache PHP-Websites
- Nextcloud/WebDAV in geeigneter Konfiguration

**Eher auf einen VPS:**

- Docker-/Container-Anwendungen
- MinIO/S3-Server
- Jellyfin/Plex/Emby-Server
- Immich/PhotoPrism als Application Server
- eigene Netzwerkdienste
- datenbank- oder IOPS-intensive Anwendungen

## Artikel

- [Was kann direkt auf der StorageBox laufen?](was-laeuft-direkt.md)
- [Nextcloud](nextcloud.md)
- [WebDAV](webdav.md)

## Offizielle Grundlage

HostBrr beschreibt StorageBoxen als geeignet für Backups, große Mediendateien und Personal-Cloud-Syncing. Gleichzeitig weist HostBrr darauf hin, dass datenbankintensive Anwendungen auf dem HDD-basierten Storage langsamer sind als auf reinem NVMe-Hosting.

https://hostbrr.com/storageboxes.html
