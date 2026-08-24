---
title: Proxmox-Backups auf HostBrr StorageBox
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# Proxmox-Backups auf HostBrr StorageBox

Für Proxmox muss zwischen zwei sehr unterschiedlichen Ansätzen unterschieden werden.

## Empfehlung: fertige vzdump-Backups übertragen

Die robuste Variante ist:

```text
Proxmox VE
   |
   | vzdump / normales PVE-Backup
   v
lokaler Backup-Speicher
   |
   | rclone oder rsync
   v
HostBrr StorageBox
```

Damit bleibt die StorageBox ein Offsite-Dateiziel. Proxmox muss keine speziellen Dateisystemeigenschaften über einen FUSE-/SFTP-Mount voraussetzen.

### Vorteile

- einfache Fehlerdomänen
- lokale Sicherung ist unabhängig von Internet/StorageBox verfügbar
- Übertragung kann nach erfolgreichem lokalen Backup erfolgen
- rclone crypt ermöglicht clientseitige Verschlüsselung
- Restore kann zunächst lokal zurückkopiert und anschließend normal über Proxmox durchgeführt werden

## PBS-Datastore auf gemounteter StorageBox

Technisch kann man versuchen, SFTP-Speicher per SSHFS/rclone/ähnlichem Mount einzubinden und darauf einen PBS-Datastore zu legen. Das ist für diese KB derzeit **nicht als Produktionslösung empfohlen**.

Community-Diskussionen weisen auf den zusätzlichen Mount-Layer als Fehlerpunkt hin. PBS ist stärker von konsistenten Dateisystem- und Metadatenoperationen abhängig als ein einfacher Transfer fertiger Backup-Dateien.

## Geplanter Praxistest

1. PVE erzeugt lokales vzdump-Backup.
2. SHA-256 des lokalen Archivs erfassen.
3. Upload mit rclone über SFTP, optional über `crypt`.
4. Remote-Datei mit `rclone check` bzw. geeignetem Integritätscheck prüfen.
5. Backup auf ein separates Restore-Ziel zurückladen.
6. Integrität erneut prüfen.
7. Test-Restore einer VM/LXC durchführen.
8. Laufzeiten und Speicherverbrauch dokumentieren.

## Noch offen

- optimale Retention lokal vs. remote
- Bandbreitenlimit für produktive Uploads
- Verhalten bei Verbindungsabbruch
- große VM-Archive
- viele LXC-Backups
- automatisierte Benachrichtigung bei fehlgeschlagenem Offsite-Transfer
- PBS experimentell separat testen

## Quellen

- https://lowendtalk.com/discussion/218682/4tb-storage-vps-or-pbs
- https://lowendtalk.com/discussion/205325/hostbrr-storage-boxes-any-experiences-with-them-good-or-bad
- https://lowendtalk.com/discussion/206272/hostbrr-storage-box-how-safe-is-it
