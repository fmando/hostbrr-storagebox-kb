---
title: QNAP NAS auf HostBrr sichern
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# QNAP NAS auf HostBrr sichern

QNAPs **Hybrid Backup Sync (HBS 3)** unterstützt Remote-Server als Speicherziele, darunter rsync-Server und FTP/FTPS. Für HostBrr ist rsync der interessanteste native Ansatz, muss aber mit der konkreten StorageBox praktisch verifiziert werden.

## Zielbild

```text
QNAP NAS
  ↓ HBS 3
rsync / verschlüsselte Verbindung
  ↓
HostBrr StorageBox
```

Offizielle Dokumentation:

- QNAP HBS 3 Schnellstart: https://www.qnap.com/de-de/how-to/tutorial/article/hbs-hybrid-backup-sync-schnellstartanleitung
- Remote-Server als Speicherplatz: https://docs.qnap.com/application/hybrid-backup-sync/3v21.x/de-de/einen-speicherplatz-auf-einem-remote-server-erstellen-9B6F1BA0.html

## Storage Space anlegen

In HBS 3 wird zunächst ein Remote-Speicherplatz angelegt. Benötigt werden je nach Modus:

- Hostname/IP
- Port
- Benutzer
- Passwort bzw. unterstützte Authentifizierung
- Zielpfad/rsync-Konfiguration

HBS besitzt einen Verbindungstest und einen Speed-Test. Diese Messwerte sind später auch für unseren Performancevergleich interessant.

## Backup statt Sync

Ein unidirektionaler Sync ist nicht automatisch ein versioniertes Backup. Für wichtige Daten sollte ein HBS-Backupjob mit geeigneter Versionierung/Retention bevorzugt werden, sofern die gewählte Zielart dies unterstützt.

## Verschlüsselung

Wenn sensible Daten auf einer Shared-Hosting-StorageBox liegen, sollte die Sicherung möglichst clientseitig verschlüsselt werden. Reine Transportverschlüsselung schützt nicht die ruhenden Daten vor einem kompromittierten Storage-Account oder Serverzugriff.

## Alternative: rclone/restic im Container

Auf leistungsfähigeren QNAP-Systemen kann ein Container oder eine andere kontrollierte Laufzeitumgebung mit rclone/restic flexibler sein als HBS. Das ist jedoch deutlich geräteabhängiger und wird als fortgeschrittene Variante separat behandelt.

## Restore-Test

Nach Einrichtung mindestens:

1. Testordner sichern.
2. Datei auf dem NAS verändern/löschen.
3. ältere Version zurückholen.
4. Prüfen, ob Dateinamen, Zeitstempel und Inhalte korrekt sind.

## Noch zu verifizieren

Wir prüfen später mit einer echten HostBrr-Box, welchen rsync-Modus HBS gegen die StorageBox zuverlässig akzeptiert und wie sich ein abweichender SSH-Port konfigurieren lässt.
