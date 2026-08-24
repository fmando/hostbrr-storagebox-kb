---
title: Mounts
category: mounts
status: maintained
last_reviewed: 2026-08-25
---
# Mounts

Die StorageBox kann als Dateisystem oder Laufwerk eingebunden werden. Ein Mount ist jedoch ein anderer Anwendungsfall als ein Backup.

## Howtos

- [Linux](linux.md) – SSHFS/rclone-basierte Einbindung
- [Windows mit SSHFS](windows-sshfs.md)
- [Cloud-Laufwerk mit lokalem Cache](../09-rezepte/cloud-drive-cache.md)
- [Troubleshooting für Transfers & Mounts](../08-troubleshooting/performance-mounts.md)

## Wichtige Grenzen

Latenz und viele Metadatenoperationen können Remote-Mounts deutlich stärker beeinträchtigen als reine Dateiübertragungen. Lokaler VFS-/SSD-/NVMe-Cache kann helfen, ändert aber nichts daran, dass das Backend remote ist.

Datenbanken, VM-Datastores oder andere Workloads mit hohen Anforderungen an Dateisystemsemantik sollten nicht ungeprüft auf einen SFTP/FUSE-Mount gelegt werden.

## Sicherheitsregel

Ein Mount macht Remote-Daten für Anwendungen direkt veränderbar. Er ersetzt weder Versionierung noch eine getrennte Backupkopie.
