---
title: Restic
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# Restic mit der HostBrr StorageBox

[Restic](https://restic.net/) ist ein verschlüsselndes Backup-Programm, das unter anderem SFTP als Backend unterstützt.

Für die HostBrr StorageBox ist das besonders interessant, weil SFTP zu den naheliegenden Zugriffswegen gehört und Restic dadurch **kein serverseitig installiertes Restic** benötigt.

## SFTP-Repository

Die offizielle Restic-Dokumentation beschreibt SFTP-Repositories direkt:

[Restic – SFTP Backend](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#sftp)

Ein Repository kann konzeptionell so adressiert werden:

```text
sftp:<USER>@<HOST>:<PFAD>
```

Bei einem abweichenden SSH-Port empfiehlt sich eine SSH-Konfiguration, zum Beispiel:

```sshconfig
Host hostbrr-storagebox
    HostName <HOST>
    User <USER>
    Port <PORT>
    IdentityFile ~/.ssh/id_ed25519
```

Dann kann Restic den SSH-Alias verwenden.

## Repository initialisieren

```bash
export RESTIC_REPOSITORY='sftp:hostbrr-storagebox:backups/restic'
restic init
```

Das Passwort sollte nicht dauerhaft ungeschützt in Shell-History oder Skripten stehen. Restic unterstützt unter anderem Passwortdateien und weitere Möglichkeiten zur Übergabe von Zugangsinformationen.

## Backup

```bash
restic backup /daten
```

## Snapshots anzeigen

```bash
restic snapshots
```

## Restore

```bash
restic restore latest --target /restore
```

Ein Backup gilt erst dann als belastbar, wenn ein Restore praktisch getestet wurde.

## Integritätsprüfung

```bash
restic check
```

Die Häufigkeit und Tiefe der Prüfungen sollte zur Größe des Repositories und zur verfügbaren Bandbreite passen.

## HostBrr-spezifische Einordnung

Vorteil gegenüber Borg: Für SFTP muss auf der StorageBox selbst kein Restic-Prozess vorhanden sein. Das reduziert die Abhängigkeit von den auf dem Shared-Hosting-System installierten Programmen.

Offen bleiben Performance und Verhalten unter den jeweiligen HostBrr-Ressourcenlimits. Deshalb ist der Status bis zu eigenen Tests `community-reported`.

## Weiterführende Dokumentation

- [Restic](https://restic.net/)
- [Restic Documentation](https://restic.readthedocs.io/)
- [Preparing a new repository / SFTP](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#sftp)
- [Restoring from backup](https://restic.readthedocs.io/en/stable/050_restore.html)
- [Checking integrity and consistency](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#checking-integrity-and-consistency)
