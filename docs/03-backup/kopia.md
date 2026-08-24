---
title: Kopia
category: backup
status: research
last_reviewed: 2026-08-24
---
# Kopia mit der HostBrr StorageBox

[Kopia](https://kopia.io/) ist ein modernes Backupwerkzeug mit Verschlüsselung, Deduplizierung, Kompression, Snapshots und Retention. Es ist deshalb als zusätzliche Alternative zu Restic und Borg interessant.

## Warum Kopia für HostBrr interessant ist

Kopia unterstützt unter anderem SFTP als Repository-Storage. Damit passt es grundsätzlich zum Zugangsmodell einer HostBrr StorageBox, ohne dass auf der StorageBox ein dauerhaft laufender Kopia-Dienst notwendig sein muss.

Offizielle Dokumentation:

- Projekt: https://kopia.io/
- Repositories: https://kopia.io/docs/repositories/
- SFTP Storage: https://kopia.io/docs/reference/command-line/common/repository-create-sftp/

## Community-Hinweis

In einer Reddit-Diskussion über Alternativen zur Hetzner Storage Box berichtet ein Nutzer, HostBrr für Backups mit Kopia eingesetzt zu haben. Das ist ein interessanter Praxisbeleg, aber noch keine reproduzierbare Verifikation unserer aktuellen 2-TB-/8-TB-Generation.

Status daher: **community-reported / noch zu verifizieren**.

## Beispielkonzept

```text
Server / PC
   ↓ Kopia
verschlüsselte Snapshots
   ↓ SFTP
HostBrr StorageBox
```

## Warum nicht einfach rclone?

rclone ist hervorragend für Dateiübertragung und mit `crypt` für verschlüsselte Kopien. Kopia ist dagegen ein eigentliches Backup-System mit Snapshot-Historie, Deduplizierung und Retention.

## Vergleich zu Restic und Borg

| Eigenschaft | Kopia | Restic | Borg |
|---|---|---|---|
| clientseitige Verschlüsselung | ja | ja | ja |
| Snapshots | ja | ja | ja |
| Deduplizierung | ja | ja | ja |
| SFTP-Ziel | ja | ja | über SSH/Borg-Remote |
| Serverprogramm auf StorageBox nötig | nein bei SFTP | nein | für effizientes Remote-Borg typischerweise ja |
| GUI | KopiaUI verfügbar | Drittanbieter | Drittanbieter |

## Für die spätere Praxisphase

Auf beiden verfügbaren HostBrr-Boxen prüfen:

1. Repository über SFTP anlegen.
2. 10–50 GB Testdaten sichern.
3. zweites inkrementelles Backup.
4. Snapshot-Liste prüfen.
5. einzelne Datei wiederherstellen.
6. vollständigen Testordner wiederherstellen.
7. Performance und Storageverbrauch mit Restic vergleichen.

Erst danach sollte Kopia in der Kompatibilitätsmatrix den Status `verified` erhalten.
