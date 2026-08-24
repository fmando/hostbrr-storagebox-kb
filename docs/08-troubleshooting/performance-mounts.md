---
title: Langsame Transfers & instabile Mounts
category: troubleshooting
status: maintained
last_reviewed: 2026-08-24
---
# Langsame Transfers & instabile Mounts

## Nicht nur einen Speedtest betrachten

Eine StorageBox kann bei einer großen Datei schnell und bei tausenden kleinen Dateien deutlich langsamer wirken. Ursache sind dann häufig Metadatenoperationen, Latenz und viele einzelne SFTP-Anfragen statt reine Bandbreite.

Deshalb getrennt testen:

1. eine große Datei
2. viele kleine Dateien
3. Upload
4. Download
5. Einzelstream
6. mehrere parallele Transfers

## Baseline ohne Mount

Vor SSHFS/rclone mount zunächst direkten SFTP-/rclone-Transfer testen. Ist bereits dieser langsam, liegt das Problem nicht am Mount-Layer.

Beispiel:

```bash
rclone -vv copy test.bin hostbrr:test/
rclone -vv copy hostbrr:test/test.bin ./download/
```

## rclone mount

Für Anwendungen mit vielen wiederholten Zugriffen kann VFS-Caching helfen. Caching verändert aber Semantik, lokalen Speicherbedarf und Fehlerbilder. Daher Cache-Parameter dokumentieren und nicht blind aus fremden Howtos übernehmen.

Offizielle Dokumentation:
- [rclone mount](https://rclone.org/commands/rclone_mount/)
- [rclone VFS](https://rclone.org/commands/rclone_mount/#vfs-file-caching)

## SSHFS

Bei kurzen Netzwerkunterbrechungen können SSHFS-Mounts hängen oder getrennt werden. Für kritische Backupjobs ist ein direktes Backup-Protokoll meist robuster als ein dauerhaft gemountetes Remote-Dateisystem.

Offizielle Dokumentation:
- [SSHFS](https://github.com/libfuse/sshfs)

## 10 Gbit/s richtig einordnen

Eine Server-/Netzwerkangabe ist kein garantierter Durchsatz eines einzelnen Shared-Hosting-Accounts. Limitierende Faktoren können Client-Uplink, Internetroute, Latenz, CPU/Verschlüsselung, Shared-I/O, IOPS, Dateigröße und accountbezogene Limits sein.

DirectAdmin unterstützt technisch Limits für CPU, RAM, I/O, IOPS und Tasks. Ob und welche davon HostBrr nutzt, muss für die konkrete StorageBox-Generation separat belegt werden.
