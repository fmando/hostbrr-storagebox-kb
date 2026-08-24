---
title: Backup-Kompatibilitätsmatrix
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# Backup-Kompatibilitätsmatrix

Diese Matrix trennt offizielle HostBrr-Funktionen von Community-Erfahrungen. `community-reported` bedeutet ausdrücklich nicht, dass wir die Kombination bereits selbst getestet haben.

| Werkzeug / Verfahren | Transport / Backend | Evidenz | Einschätzung | Hinweise |
|---|---|---|---|---|
| rsync | SSH | offiziell + Community | sehr gut geeignet | HostBrr bewirbt rsync; Community nutzt automatisierte Backupjobs |
| rclone | SFTP | Community mehrfach | sehr gut geeignet | häufigste Community-Lösung; auch mit `crypt` und Mounts |
| rclone crypt | SFTP | Community mehrfach | sehr gut geeignet | clientseitige Verschlüsselung; Dateigröße und ungefähre Pfadlängen können Metadaten leaken |
| Borg | SSH / serverseitiges Borg | Community + ältere Provider-Aussage | vielversprechend | 2024 wurde Borg 1.2.4 als installiert bestätigt; aktuelle Version/Verfügbarkeit muss geprüft werden |
| Restic | SFTP | Community | funktioniert, Performance uneinheitlich | ein Nutzer berichtete 2025 deutlich bessere Performance mit Borg als mit Restic |
| SSHFS | SSH/SFTP | Community | geeignet für Mounts | eher Mount als Backupformat; Latenz und viele Metadatenoperationen beachten |
| rclone mount | SFTP + VFS Cache | Community mehrfach | geeignet | VFS-Cache kann Performance deutlich verbessern |
| JuiceFS | SFTP Backend | Community | fortgeschritten | Community-Bericht nennt bessere Metadatenperformance als rclone mount bei vielen kleinen Dateien |
| Proxmox Backup Server | gemountetes SFTP/SSHFS/rclone Backend | Community diskutiert | nicht empfohlen ohne Tests | zusätzlicher Mount-Layer ist ein Fehlerpunkt; PBS erwartet robuste Storage-Semantik |
| Proxmox vzdump-Dateien | rsync/rclone/SFTP | technisch naheliegend | gut geeignet | fertige vzdump-Dateien offsite übertragen statt PBS-Datastore auf FUSE-Mount zu erzwingen |

## Priorität für eigene Tests

1. rclone über SFTP
2. rclone crypt
3. rsync über SSH
4. Borg: installierte Version und Remote-Repository testen
5. Restic über SFTP
6. Restore-Test aller Backupverfahren
7. Proxmox: vzdump-Dateien per rclone/rsync übertragen
8. erst danach experimentell PBS über einen Mount-Layer untersuchen

## Quellen

- https://hostbrr.com/storageboxes.html
- https://lowendtalk.com/discussion/206272/hostbrr-storage-box-how-safe-is-it
- https://lowendtalk.com/discussion/205325/hostbrr-storage-boxes-any-experiences-with-them-good-or-bad
- https://lowendtalk.com/discussion/199617/hostbrr-bf-storage-deals-epyc-10-gbps-storage-vps-directadmin-storage-boxes-500gb-7-year/p10
- https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p12
- https://lowendtalk.com/discussion/192011/hostbrr-directadmin-cpanel-hosting-reseller-100-nvme-litespeed-eu-usa-sg-upto-50-off/p25
- https://lowendtalk.com/discussion/218682/4tb-storage-vps-or-pbs
