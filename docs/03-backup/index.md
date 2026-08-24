---
title: Backup
category: backup
status: maintained
last_reviewed: 2026-08-25
---
# Backup

Die StorageBox eignet sich besonders als Offsite-Ziel. Entscheidend ist aber, ob eine einfache Kopie, ein Mirror oder ein versioniertes Backup benötigt wird.

## Einstieg

- [Welche Backup-Methode ist die richtige?](welche-backup-methode.md)
- [Backup-Kompatibilitätsmatrix](kompatibilitaetsmatrix.md)

## Werkzeuge

- [rsync](rsync.md) – transparente Kopien und Mirrors
- [rclone + SFTP + crypt](rclone-sftp-crypt.md) – verschlüsselte Offsite-Kopien
- [Restic](restic.md) – versionierte, verschlüsselte Backups über SFTP
- [Kopia](kopia.md) – Snapshots, Policies, Deduplizierung und SFTP
- [BorgBackup](borg.md) – leistungsfähig, aber serverseitige Kompatibilität prüfen
- [Proxmox](proxmox.md) – Einordnung für vzdump und PBS

## Wichtige Unterscheidung

Transportverschlüsselung schützt die Verbindung. Clientseitige Verschlüsselung schützt zusätzlich die auf der StorageBox abgelegten Inhalte.

Eine Synchronisation mit `rsync --delete` oder `rclone sync` ist außerdem nicht automatisch ein versioniertes Backup.

Für konkrete Schritt-für-Schritt-Anleitungen siehe [Rezepte & Howtos](../09-rezepte/index.md).
