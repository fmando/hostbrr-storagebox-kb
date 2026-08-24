---
title: Was kann direkt auf der StorageBox laufen?
category: anwendungen
status: community-reported
last_reviewed: 2026-08-24
---
# Was kann direkt auf der StorageBox laufen?

Die wichtigste Grenze ist das Betriebsmodell: HostBrr liefert eine StorageBox mit DirectAdmin, LiteSpeed und PHP, aber keinen Root-VPS.

Offizielle Produktbeschreibung: https://hostbrr.com/storageboxes.html

## Gut geeignet

### Backup- und Archivziel

Sehr naheliegender Einsatzzweck. SSH/SFTP, rsync und Werkzeuge wie rclone, Restic oder – sofern serverseitig kompatibel – Borg passen zum Storage-Charakter.

### Statische Webseiten und Downloads

HostBrr nennt Standard-Websites, Blogs und statische Inhalte ausdrücklich als mögliche Nutzung. Große Downloads sollten hinsichtlich Transferkontingent und Zugriffsprofil geplant werden.

### Persönliche Dateiablage

SFTP/FTP und ein vorgeschalteter Client wie rclone eignen sich gut, wenn keine vollständige Web-Cloud benötigt wird.

### Nextcloud / WebDAV mit Einschränkungen

HostBrr nennt Personal-Cloud-Syncing als Einsatzzweck. In der Community wird Nextcloud über Softaculous häufig als Weg zu WebDAV verwendet.

Nextcloud: https://nextcloud.com/
Nextcloud Admin Manual: https://docs.nextcloud.com/server/latest/admin_manual/

Allerdings gibt es Community-Berichte über PHP-/OPcache-Konfigurationen, die bei einzelnen StorageBox-Generationen Nextcloud-Warnungen erzeugten. Daher ist Nextcloud direkt auf der Box als `community-reported` und versionsabhängig zu behandeln.

## Besser auf einem VPS, StorageBox nur als Speicher/Backup

### Rechenintensive Nextcloud-Installationen

Für größere Benutzerzahlen, Apps, Preview-Generierung, Office-Integration oder datenbankintensive Workloads ist ein VPS mit eigener Kontrolle über PHP, Redis, Datenbank und Background Jobs meist sauberer.

### Photo- und Media-Anwendungen

Immich, PhotoPrism und ähnliche Anwendungen benötigen zusätzliche Dienste, Datenbanken, Worker oder Container. Die StorageBox eignet sich hier eher als Daten-/Backupziel als als eigentlicher Application Server.

Immich: https://immich.app/
PhotoPrism: https://www.photoprism.app/

### Jellyfin/Plex/Emby

Die Mediendateien können auf Remote-Storage liegen, der Media Server selbst gehört wegen dauerhaft laufender Prozesse, Datenbank, Metadaten und Transcoding auf einen VPS/Server.

Jellyfin: https://jellyfin.org/
Plex: https://www.plex.tv/
Emby: https://emby.media/

### S3/MinIO

MinIO benötigt einen dauerhaft laufenden Dienst und einen erreichbaren Port. Community-Berichte beschreiben genau diese Möglichkeit auf den DirectAdmin-StorageBoxen als nicht verfügbar.

MinIO: https://www.min.io/

Eine Alternative ist, MinIO/S3-Kompatibilität auf einem VPS bereitzustellen und die StorageBox dahinter anzubinden – allerdings entstehen dabei zusätzliche Schichten und mögliche Performance-/Konsistenzprobleme.

## Nicht mit einem VPS verwechseln

Folgende Ideen aus Forendiskussionen sind nicht automatisch praktikabel, nur weil eine Shell vorhanden ist:

- Docker/Podman-Daemons
- eigene dauerhaft laufende Netzwerkdienste
- WireGuard/OpenVPN-Server
- iSCSI-Target
- Samba/NFS-Server auf eigenen Ports
- MinIO direkt
- Jellyfin/Plex-Server direkt
- Datenbanken mit hohen IOPS-Anforderungen

## Entscheidungsregel

Wenn eine Anwendung nur Dateien über vorhandene Protokolle benötigt, ist die StorageBox häufig gut geeignet.

Wenn sie dagegen Root-Rechte, Container, zusätzliche Ports, Systemdienste, Redis, Worker, hohe Datenbankleistung oder dauerhaft laufende Prozesse benötigt, sollte die Anwendung auf einem VPS laufen und die StorageBox nur Speicher- oder Backupfunktion übernehmen.
