---
title: Windows-PC verschlüsselt sichern
category: rezepte
status: maintained
last_reviewed: 2026-08-24
---
# Windows-PC verschlüsselt auf die StorageBox sichern

## Ziel

Persönliche Dateien eines Windows-PCs sollen automatisiert, versioniert und clientseitig verschlüsselt auf einer HostBrr StorageBox gesichert werden.

Für echte versionierte Backups empfehlen wir **Restic über SFTP**. Für eine reine verschlüsselte Kopie ohne Snapshot-Historie ist alternativ **rclone crypt** geeignet.

## Architektur

```text
Windows-PC
  |
  | Restic / SFTP
  | clientseitig verschlüsselt
  v
HostBrr StorageBox
  `-- backups/windows-pc/restic/
```

## Voraussetzungen

- SSH/SFTP-Zugang zur StorageBox
- SSH-Key empfohlen
- Restic auf Windows
- sicher verwahrtes Repository-Passwort
- ausreichend freier Speicher auf der StorageBox

Offizielle Dokumentation:

- [Restic](https://restic.net/)
- [Restic – Repository vorbereiten](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html)
- [Restic – Restore](https://restic.readthedocs.io/en/stable/050_restore.html)
- [OpenSSH für Windows](https://learn.microsoft.com/windows-server/administration/openssh/openssh-overview)

## Repository initialisieren

PowerShell-Beispiel:

```powershell
$env:RESTIC_REPOSITORY = "sftp:user@storagebox:/home/user/backups/windows-pc/restic"
$env:RESTIC_PASSWORD_FILE = "C:\Secure\restic-password.txt"
restic init
```

Host, Benutzer, Port und Zielpfad müssen an die eigene HostBrr-Box angepasst werden. Die tatsächlich ermittelte Home-Verzeichnisstruktur hat Vorrang vor Beispielpfaden.

## Backup

```powershell
restic backup `
  "C:\Users\NAME\Documents" `
  "C:\Users\NAME\Pictures"
```

Nicht blind das komplette Windows-System sichern. Browser-Caches, temporäre Dateien, Download-Caches und andere reproduzierbare Daten sollten ausgeschlossen werden.

## Prüfen

```powershell
restic snapshots
restic stats
restic check
```

Für wichtige Backups reicht ein erfolgreicher Exit-Code allein nicht. Repository-Prüfung und regelmäßige Restore-Tests gehören zum Verfahren.

## Aufbewahrung

Beispiel:

```powershell
restic forget --keep-daily 7 --keep-weekly 5 --keep-monthly 12
```

`prune` sollte bewusst geplant und nicht zwingend bei jedem Backup ausgeführt werden.

## Automatisierung

Unter Windows eignet sich die **Aufgabenplanung**. Ein produktiver Job sollte:

1. die benötigten Umgebungsvariablen setzen,
2. Restic starten,
3. den Exit-Code prüfen,
4. ein Log schreiben,
5. bei Fehlern eine Benachrichtigung auslösen.

Das Passwort sollte nicht direkt als Klartextargument in der Aufgabenplanung hinterlegt werden. Eine geschützte Passwortdatei mit restriktiven NTFS-Rechten ist besser.

## Restore-Test

```powershell
New-Item -ItemType Directory -Force C:\Restore-Test
restic restore latest --target C:\Restore-Test
```

Danach nicht nur prüfen, ob Dateien vorhanden sind: exemplarisch Dokumente, Bilder und andere wichtige Dateien öffnen und vergleichen.

Für einen echten PC-Verlust lautet der Rückweg:

1. Windows bzw. Ersatz-PC bereitstellen,
2. Restic und SSH-Zugang einrichten,
3. Repository-Passwort wiederherstellen,
4. Snapshot auswählen,
5. zunächst in ein separates Verzeichnis restaurieren,
6. Daten prüfen,
7. anschließend an die gewünschten Zielorte kopieren.

## Sicherheit

Das Restic-Passwort darf weder im Git-Repository noch in öffentlich lesbaren Skripten landen. Ohne Passwort ist das verschlüsselte Repository nicht wiederherstellbar.

SSH-Key und Restic-Passwort sollten getrennt gesichert werden. Die StorageBox ist außerdem nicht als einzige Kopie wichtiger Daten gedacht.

Weiterlesen:

- [SSH-Key-Härtung](../06-sicherheit/ssh-key-haertung.md)
- [Verschlüsselung](../06-sicherheit/verschluesselung.md)
- [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md)
- [Disaster Recovery](disaster-recovery.md)

## Typische Fehler

### SFTP-Verbindung funktioniert interaktiv, aber nicht automatisch

Häufig wartet SSH auf Passwort, Host-Key-Bestätigung oder eine Passphrase. Automatisierte Jobs müssen ohne interaktive Rückfragen funktionieren.

### Repository-Passwortdatei wird nicht gefunden

Bei Aufgabenplanung und interaktiver PowerShell können unterschiedliche Benutzerkontexte und Arbeitsverzeichnisse gelten. Absolute Pfade verwenden.

### Backup wird immer größer

Prüfen, ob große temporäre Verzeichnisse, VM-Images, Downloads oder andere ständig veränderte Daten versehentlich enthalten sind.

### Backup vorhanden, Restore nie getestet

Das ist kein technischer Fehler, aber ein Betriebsrisiko. Mindestens einzelne Dateien und periodisch einen größeren Verzeichnisbaum zurückspielen.

## HostBrr-spezifisch noch zu verifizieren

- aktueller SFTP-Port und Pfadaufbau der eigenen Box
- Verhalten langer Windows-Dateinamen über SFTP
- Performance mit vielen kleinen Dateien
- sinnvolle Parallelität und Repository-Größe
- Unterschiede zwischen 2-TB- und 8-TB-Box
