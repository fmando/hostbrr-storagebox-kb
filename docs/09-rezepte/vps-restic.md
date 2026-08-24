---
title: VPS verschlüsselt mit Restic sichern
category: rezepte
status: maintained
last_reviewed: 2026-08-24
---
# VPS verschlüsselt mit Restic auf HostBrr sichern

## Ziel

Ein Linux-VPS soll automatisiert, versioniert und clientseitig verschlüsselt auf eine HostBrr StorageBox gesichert werden.

## Warum Restic?

Restic unterstützt SFTP als Repository-Ziel. Dadurch muss Restic nicht auf der StorageBox installiert sein. Verschlüsselung und Snapshot-Verwaltung passieren auf dem VPS.

Offizielle Dokumentation:

- [Restic – Preparing a new repository](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html)
- [Restic – Backing up](https://restic.readthedocs.io/en/stable/040_backup.html)
- [Restic – Restoring](https://restic.readthedocs.io/en/stable/050_restore.html)

## Architektur

```text
Linux VPS
   |
   | Restic / SFTP
   | clientseitig verschlüsselt
   v
HostBrr StorageBox
   `-- backups/vps-name/restic/
```

## Voraussetzungen

- SSH/SFTP-Zugang zur StorageBox
- bevorzugt SSH-Key
- Restic auf dem zu sichernden VPS
- separates Restic-Passwort

```bash
apt update
apt install restic
```

## Repository

Beispielvariablen:

```bash
export RESTIC_REPOSITORY='sftp:USER@HOST:/home/USER/backups/vps01/restic'
export RESTIC_PASSWORD_FILE='/root/.config/restic/password'
```

Bei HostBrr können Hostname, Port und Pfad von diesem Beispiel abweichen. Welcome-Mail und tatsächlich ermittelte `$HOME`-Struktur haben Vorrang.

Repository initialisieren:

```bash
restic init
```

## Erstes Backup

Beispiel:

```bash
restic backup /etc /home /root /var/www
```

Datenbanken sollten konsistent gedumpt oder mit anwendungsspezifischen Backupmethoden gesichert werden, bevor Restic die Dump-Dateien übernimmt.

## Snapshots prüfen

```bash
restic snapshots
restic stats
```

## Retention

Beispiel:

```bash
restic forget \
  --keep-daily 7 \
  --keep-weekly 5 \
  --keep-monthly 12
```

`prune` sollte bewusst geplant werden, weil es ein Remote-Repository stärker belastet:

```bash
restic prune
```

## Restore-Test

Ein Backup gilt erst als belastbar, wenn der Restore getestet wurde:

```bash
mkdir -p /tmp/restic-restore
restic restore latest --target /tmp/restic-restore
```

Danach exemplarisch Dateien und Anwendungen prüfen.

## Automatisierung

Das Backup kann per systemd timer oder Cron ausgeführt werden. Für produktive Systeme bevorzugen wir ein Skript mit Logging, Exit-Code-Prüfung und optionaler Benachrichtigung.

## Sicherheit

Das Restic-Passwort nicht im Repository speichern. Geht das Passwort verloren, ist das verschlüsselte Backup nicht wiederherstellbar.

SSH-Key und Restic-Passwort sollten getrennt gesichert werden.

## HostBrr-spezifisch noch zu verifizieren

- sinnvoller SFTP-Verbindungsumfang
- tatsächliche Pfade der aktuellen StorageBox-Generation
- Verhalten bei großen Repositories
- optimale Retention-/Prune-Intervalle
- Unterschiede zwischen 2-TB- und 8-TB-Box
