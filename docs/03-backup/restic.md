---
title: Restic
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# Restic mit der HostBrr StorageBox

[Restic](https://restic.net/) ist für die HostBrr StorageBox eine der interessantesten Backup-Lösungen: Verschlüsselung, Snapshots, Deduplizierung und Retention laufen clientseitig, während die Box nur als SFTP-Ziel dienen muss.

## Architektur

```text
Server / PC
   ↓ Restic
verschlüsselte Snapshots
   ↓ SFTP
HostBrr StorageBox
```

Auf der StorageBox muss kein Restic-Prozess installiert sein.

## Voraussetzungen

- Restic auf dem Quellsystem
- HostBrr SSH/SFTP-Zugang
- vorzugsweise SSH-Key
- sicher verwahrtes Restic-Repository-Passwort
- ausreichend freie Quota auf der StorageBox

## SFTP konfigurieren

Bei abweichendem SSH-Port ist ein SSH-Alias praktisch:

```sshconfig
Host hostbrr-storagebox
    HostName <HOST>
    User <USER>
    Port <PORT>
    IdentityFile ~/.ssh/id_ed25519
```

Verbindung zuerst unabhängig von Restic testen:

```bash
ssh hostbrr-storagebox
sftp hostbrr-storagebox
```

## Repository initialisieren

```bash
export RESTIC_REPOSITORY='sftp:hostbrr-storagebox:backups/restic'
export RESTIC_PASSWORD_FILE='/root/.config/restic/password'
restic init
```

Die Passwortdatei sollte nur für den Backup-Benutzer lesbar sein:

```bash
chmod 600 /root/.config/restic/password
```

Das Repository-Passwort zusätzlich außerhalb des zu sichernden Systems verwahren.

## Backup

```bash
restic backup /etc /srv /home
```

Snapshots anzeigen:

```bash
restic snapshots
```

## Retention

Beispiel:

```bash
restic forget \
  --keep-daily 7 \
  --keep-weekly 5 \
  --keep-monthly 12
```

`forget` entfernt Snapshot-Referenzen. Speicherplatz wird erst durch `prune` tatsächlich zurückgewonnen:

```bash
restic prune
```

Alternativ:

```bash
restic forget --keep-daily 7 --keep-weekly 5 --keep-monthly 12 --prune
```

Bei großen Remote-Repositories sollte `prune` nicht unbedacht nach jedem Backup laufen: es kann lange dauern, sperrt das Repository und kann beim Repacking Daten herunterladen und erneut hochladen.

## Integritätsprüfung

Regelmäßig:

```bash
restic check
```

Für eine tiefere Prüfung können Repository-Daten zusätzlich gelesen werden; Umfang und Häufigkeit müssen bei Multi-TB-Repositories gegen Laufzeit und Traffic abgewogen werden.

Nach größeren Wartungs-/Prune-Läufen ist ein `restic check` besonders sinnvoll.

## Restore

Snapshots ansehen:

```bash
restic snapshots
```

Inhalt eines Snapshots untersuchen:

```bash
restic ls latest
```

Test-Restore:

```bash
mkdir -p /restore-test
restic restore latest --target /restore-test
```

Für kritische Systeme sollte nicht nur eine einzelne Datei, sondern gelegentlich ein vollständiger Restore in eine isolierte Testumgebung erfolgen.

## Automatisierung

Ein täglicher Job sollte mindestens:

1. Backup ausführen,
2. Exit-Code protokollieren,
3. Fehler melden,
4. Retention separat oder geplant ausführen,
5. regelmäßige Checks vorsehen.

Beispiel für einen einfachen Cronjob:

```cron
15 2 * * * /usr/local/sbin/backup-restic.sh >>/var/log/restic-backup.log 2>&1
```

Secrets nicht direkt in die Crontab schreiben.

## Sicherheit und Ransomware

Ein normales SFTP-Repository ist nicht automatisch append-only. Besitzt ein kompromittierter Quellserver vollständige Schreib-/Löschrechte, kann ein Angreifer grundsätzlich auch das Backup beschädigen oder löschen.

Deshalb bleibt eine zusätzliche unabhängige Kopie beziehungsweise ein getrenntes Sicherheitsmodell wichtig. Siehe auch:

- [Bedrohungsmodell](../06-sicherheit/bedrohungsmodell.md)
- [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md)
- [Ransomware & Löschschutz](../06-sicherheit/ransomware-loeschschutz.md)

## Typische Fehler

### SFTP funktioniert, Restic aber nicht

Repository-URL, relativen Zielpfad und SSH-Alias prüfen. Restic benötigt keinen serverseitigen Restic-Befehl.

### Repository fast voll

Nicht sofort aggressiv `prune` starten. Zuerst Snapshots und Quota prüfen. Prune benötigt selbst Arbeits-/Repack-Spielraum und kann bei einem praktisch vollen Remote-Repository problematisch werden.

### Backup läuft, Restore wurde nie getestet

Das ist kein technischer Fehler, aber ein Betriebsrisiko. Restore-Test fest in den Wartungsplan aufnehmen.

## HostBrr-spezifisch noch zu testen

Auf 2-TB- und 8-TB-Box identisch prüfen:

- Repository anlegen
- 10–50 GB Erstbackup
- inkrementelles Folgebackup
- viele kleine Dateien
- `snapshots`, `check`, `forget`, `prune`
- Einzeldatei-Restore
- vollständiger Test-Restore
- Verhalten nahe der Quota
- Performance während `prune`

## Einordnung

**Stärken:** echtes Backup-System, kein Restic auf HostBrr erforderlich, gute Verschlüsselung, Snapshots und einfache CLI.

**Schwächen:** Wartung großer Remote-Repositories kann I/O und Traffic erzeugen; SFTP allein bietet keinen automatischen Löschschutz gegen einen kompromittierten Backup-Client.

## Primärquellen

- https://restic.net/
- https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#sftp
- https://restic.readthedocs.io/en/stable/040_backup.html
- https://restic.readthedocs.io/en/stable/045_working_with_repos.html
- https://restic.readthedocs.io/en/stable/050_restore.html
- https://restic.readthedocs.io/en/stable/060_forget.html
