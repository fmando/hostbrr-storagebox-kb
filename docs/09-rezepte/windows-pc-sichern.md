---
title: Windows-PC verschlüsselt sichern
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Windows-PC verschlüsselt auf die StorageBox sichern

Für einen Windows-PC ist **Restic über SFTP** eine robuste Variante, wenn echte versionierte Backups gewünscht sind. Für eine reine verschlüsselte Kopie eignet sich alternativ **rclone crypt**.

## Zielbild

```text
Windows-PC
  ↓ Restic
verschlüsseltes, versioniertes Repository
  ↓ SFTP
HostBrr StorageBox
```

## Voraussetzungen

- SSH/SFTP-Zugang zur StorageBox
- SSH-Key empfohlen
- Restic auf Windows
- sicher verwahrtes Repository-Passwort

Offizielle Dokumentation:

- Restic: https://restic.net/
- Restic-Dokumentation: https://restic.readthedocs.io/
- OpenSSH für Windows: https://learn.microsoft.com/windows-server/administration/openssh/openssh-overview

## Repository initialisieren

PowerShell-Beispiel:

```powershell
$env:RESTIC_REPOSITORY = "sftp:user@storagebox:/home/user/restic/windows-pc"
$env:RESTIC_PASSWORD_FILE = "C:\Secure\restic-password.txt"
restic init
```

Host, Benutzer, Port und Zielpfad müssen an die eigene HostBrr-Box angepasst werden.

## Backup

```powershell
restic backup `
  "C:\Users\NAME\Documents" `
  "C:\Users\NAME\Pictures"
```

Nicht blind das komplette Windows-System sichern. Browser-Caches, temporäre Dateien und andere reproduzierbare Daten sollten ausgeschlossen werden.

## Snapshots prüfen

```powershell
restic snapshots
restic stats
restic check
```

## Aufbewahrung

Beispiel:

```powershell
restic forget --keep-daily 7 --keep-weekly 5 --keep-monthly 12
```

`prune` sollte bewusst und nicht zwingend bei jedem Backup ausgeführt werden.

## Restore testen

```powershell
mkdir C:\Restore-Test
restic restore latest --target C:\Restore-Test
```

Ein Backup gilt erst dann als brauchbar, wenn ein Restore praktisch funktioniert.

## Geheimnisse

Das Restic-Passwort darf weder im Git-Repository noch in öffentlich lesbaren Skripten landen. Ohne Passwort kann ein verschlüsseltes Restic-Repository nicht wiederhergestellt werden.
