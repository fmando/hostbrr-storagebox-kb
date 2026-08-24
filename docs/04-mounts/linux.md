---
title: Linux – StorageBox mounten
category: mounts
status: community-reported
last_reviewed: 2026-08-25
---
# HostBrr StorageBox unter Linux mounten

Unter Linux sind insbesondere SSHFS und rclone mount interessant. Beide greifen letztlich über SSH/SFTP auf die StorageBox zu und benötigen keinen Root-Zugriff auf der HostBrr-Seite.

## Variante A: SSHFS

[SSHFS](https://github.com/libfuse/sshfs) stellt ein entferntes Dateisystem über SFTP lokal bereit.

Beispiel:

```bash
mkdir -p ~/mnt/hostbrr
sshfs -p <PORT> <USER>@<HOST>:<REMOTE_PATH> ~/mnt/hostbrr
```

Aushängen je nach System beispielsweise mit:

```bash
fusermount3 -u ~/mnt/hostbrr
```

Vor dem Mount sollte die SSH-Verbindung separat getestet werden.

## Variante B: rclone mount

Wenn bereits ein rclone-SFTP-Remote eingerichtet ist:

```bash
mkdir -p ~/mnt/hostbrr
rclone mount hostbrr: ~/mnt/hostbrr
```

Für bestimmte Workloads kann VFS-Caching sinnvoll sein. Die Auswirkungen auf lokalen Speicherverbrauch, Konsistenz und Schreibverhalten müssen jedoch verstanden werden.

Offizielle Dokumentation:

- [rclone mount](https://rclone.org/commands/rclone_mount/)
- [rclone VFS File Caching](https://rclone.org/commands/rclone_mount/#vfs-file-caching)

## Wann lieber nicht mounten?

Für reine Backups ist ein Mount oft unnötig. Werkzeuge wie rclone, rsync, Restic oder Borg arbeiten direkt mit dem Ziel und vermeiden eine zusätzliche Dateisystemschicht.

Ein Mount ist dagegen praktisch, wenn Programme tatsächlich einen normalen Dateisystempfad erwarten.

## Performance

SFTP-basierte Mounts sind besonders empfindlich gegenüber Latenz und vielen Metadatenzugriffen. Die beworbene Netzwerkgeschwindigkeit einer Storage-Plattform sagt deshalb wenig darüber aus, wie schnell ein Verzeichnis mit zehntausenden kleinen Dateien durchsucht werden kann.

## Weiterführende Dokumentation

- [SSHFS](https://github.com/libfuse/sshfs)
- [Linux Kernel – FUSE Overview](https://www.kernel.org/doc/html/latest/filesystems/fuse/fuse.html)
- [Linux Kernel – FUSE Documentation Index](https://www.kernel.org/doc/html/latest/filesystems/fuse/index.html)
- [rclone SFTP](https://rclone.org/sftp/)
- [rclone mount](https://rclone.org/commands/rclone_mount/)
