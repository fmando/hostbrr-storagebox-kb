---
title: Windows – StorageBox mit SSHFS einbinden
category: mounts
status: community-reported
last_reviewed: 2026-08-24
---
# HostBrr StorageBox unter Windows als Laufwerk einbinden

Eine HostBrr StorageBox kann über SFTP/SSHFS als Windows-Laufwerk eingebunden werden. Community-Berichte bestätigen diese Nutzung mit HostBrr; die genaue Konfiguration hängt von Hostname, Benutzer und SSH-Port der eigenen Box ab.

## SSHFS-Win

[SSHFS-Win](https://github.com/winfsp/sshfs-win) ist eine Windows-Portierung von SSHFS und verwendet [WinFsp](https://winfsp.dev/) für die Dateisystemintegration.

Die offizielle SSHFS-Win-Dokumentation beschreibt die Einbindung über den Windows Explorer oder `net use`.

## Installation

Laut Projekt können die Komponenten unter anderem über WinGet installiert werden:

```powershell
winget install WinFsp.WinFsp
winget install SSHFS-Win.SSHFS-Win
```

Anschließend sollte zuerst geprüft werden, ob der normale SSH-Zugang zur StorageBox funktioniert.

```powershell
ssh -p <PORT> <USER>@<HOST>
```

## Laufwerk über Explorer

SSHFS-Win verwendet UNC-Pfade nach dem Muster:

```text
\\sshfs\USER@HOST\PFAD
```

Bei abweichendem Port unterstützt SSHFS-Win eine Portangabe im Hostteil. Die jeweils aktuelle Syntax sollte mit der Projektdokumentation abgeglichen werden.

## Wichtiger Hinweis zu SSH-Keys

SSHFS-Win weist bei bestimmten direkten Key-Modi auf Einschränkungen bei passphrase-geschützten Schlüsseln hin. Für produktive Nutzung sollte daher vorab entschieden werden, ob Passwortauthentifizierung, SSH-Key, SSH-Agent oder ein GUI-Frontend eingesetzt wird.

## Alternative: rclone mount

[rclone mount](https://rclone.org/commands/rclone_mount/) kann ebenfalls einen zuvor eingerichteten SFTP-Remote als Dateisystem bereitstellen. Unter Windows verwendet rclone dafür typischerweise WinFsp.

Das kann insbesondere dann interessant sein, wenn ohnehin bereits ein rclone-Remote für Backup oder `rclone crypt` vorhanden ist.

## Einsatzgrenzen

Ein Remote-Mount über SFTP verhält sich nicht wie eine lokale SSD. Besonders viele kleine Dateien, Metadatenoperationen, zufällige Zugriffe und Anwendungen mit Datenbanken können deutlich langsamer sein.

Für Backupdaten ist `copy`, `sync` oder ein dediziertes Backupwerkzeug häufig robuster als ein dauerhaft gemountetes Remote-Dateisystem.

## Weiterführende Dokumentation

- [SSHFS-Win](https://github.com/winfsp/sshfs-win)
- [WinFsp](https://winfsp.dev/)
- [rclone SFTP](https://rclone.org/sftp/)
- [rclone mount](https://rclone.org/commands/rclone_mount/)
