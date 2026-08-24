---
title: rclone, Restic, Kopia, Borg & rsync Troubleshooting
category: troubleshooting
status: maintained
last_reviewed: 2026-08-25
---
# rclone, Restic, Kopia, Borg & rsync Troubleshooting

## Grundregel

Nicht sofort Optionen ändern. Zuerst feststellen, ob das Problem bei Netzwerk, SSH/SFTP, Authentifizierung, Quota, lokalem Datenträger oder Backup-Tool liegt.

## rclone

Remote prüfen:

```bash
rclone lsd hostbrr:
rclone about hostbrr:
```

Mit Diagnoseausgabe:

```bash
rclone -vv lsd hostbrr:
```

Bei `crypt` zuerst getrennt prüfen:

```bash
rclone lsd hostbrr:
rclone lsd hostbrr-crypt:
```

So lässt sich unterscheiden, ob SFTP oder die Crypt-Konfiguration betroffen ist.

Bei Integritätsprüfungen beachten: SFTP stellt nicht automatisch dieselben serverseitigen Hashes wie Object Storage bereit. Bei `crypt` ist `rclone cryptcheck` der passende Spezialfall; je nach Prüfung kann zusätzlicher Download-Traffic entstehen.

Offizielle Dokumentation:
- [rclone SFTP](https://rclone.org/sftp/)
- [rclone crypt](https://rclone.org/crypt/)
- [rclone check](https://rclone.org/commands/rclone_check/)
- [rclone cryptcheck](https://rclone.org/commands/rclone_cryptcheck/)
- [rclone logging](https://rclone.org/docs/#logging)

## Restic

Snapshots anzeigen:

```bash
restic snapshots
```

Repository-Struktur prüfen:

```bash
restic check
```

Ein `check` ist wertvoller als nur zu sehen, dass der letzte Backup-Befehl Exit-Code 0 geliefert hat.

Wenn ein abgebrochener Prozess einen Lock hinterlassen hat, zuerst sicherstellen, dass kein anderer Restic-Prozess arbeitet. Erst danach kommt `restic unlock` in Betracht.

Offizielle Dokumentation:
- [Restic – Checking integrity](https://restic.readthedocs.io/en/stable/045_working_with_repos.html#checking-integrity-and-consistency)
- [Restic Documentation](https://restic.readthedocs.io/)

## Kopia

Repository-Verbindung und Snapshots prüfen:

```bash
kopia repository status
kopia snapshot list
```

Snapshot-Inhalte verifizieren:

```bash
kopia snapshot verify
```

Für eine tiefere Prüfung kann Kopia auch einen Anteil der Dateien tatsächlich aus dem Repository lesen, entschlüsseln und dekomprimieren. Das erhöht bei einem Remote-SFTP-Ziel entsprechend I/O und Traffic.

Offizielle Dokumentation:
- [Kopia Documentation](https://kopia.io/docs/)
- [snapshot verify](https://kopia.io/docs/reference/command-line/common/snapshot-verify/)

## Borg

Bei HostBrr zuerst Versionsfrage klären:

```bash
borg --version
ssh -p <PORT> <USER>@<HOST> borg --version
```

Client- und Serverseite können bei Remote-Repositories relevant sein.

Repository prüfen:

```bash
borg check <REPOSITORY>
```

Locks niemals reflexartig entfernen. Erst ausschließen, dass noch ein Borg-Prozess auf das Repository zugreift.

Offizielle Dokumentation:
- [BorgBackup Documentation](https://borgbackup.readthedocs.io/)
- [borg check](https://borgbackup.readthedocs.io/en/stable/usage/check.html)

## rsync

Mehr Diagnose:

```bash
rsync -av --stats --progress -e "ssh -p <PORT>" <SOURCE>/ <USER>@<HOST>:<TARGET>/
```

Trockenlauf vor riskanten Änderungen:

```bash
rsync -avn --delete -e "ssh -p <PORT>" <SOURCE>/ <USER>@<HOST>:<TARGET>/
```

`--delete` nie als Troubleshooting-Experiment auf produktiven Daten verwenden.

Offizielle Dokumentation:
- [rsync project](https://rsync.samba.org/)

## Quota als versteckte Ursache

Bei Schreibfehlern, abgebrochenen Backups oder unerklärlichem Verhalten immer auch freien Speicher/Quota prüfen. Ein Netzwerkfehler und ein volles Konto können auf Anwendungsebene ähnlich aussehen.
