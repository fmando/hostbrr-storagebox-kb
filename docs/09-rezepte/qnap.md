---
title: QNAP NAS auf HostBrr sichern
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# QNAP NAS auf HostBrr sichern

## Ziel

Ein QNAP NAS soll automatisiert auf eine HostBrr StorageBox sichern. Der native Ausgangspunkt ist **Hybrid Backup Sync (HBS 3)** mit einem Remote-rsync-Ziel. Die konkrete Kompatibilität mit der aktuellen StorageBox-Generation wird später praktisch verifiziert.

## Architektur

```text
QNAP NAS
  ↓ HBS 3 / Backupjob
rsync / Remote-Verbindung
  ↓
HostBrr StorageBox
```

## Voraussetzungen

Vorher dokumentieren:

- HostBrr-Hostname
- Benutzername
- SSH-/rsync-Port
- Zielpfad
- verfügbare Quota
- HBS-3-Version
- gewünschte Retention
- Verschlüsselungsstrategie

Offizielle Dokumentation:

- QNAP HBS 3: https://www.qnap.com/de-de/software/hybrid-backup-sync
- HBS 3 Schnellstart: https://www.qnap.com/de-de/how-to/tutorial/article/hbs-hybrid-backup-sync-schnellstartanleitung
- Remote-Server als Speicherplatz: https://docs.qnap.com/application/hybrid-backup-sync/3v21.x/de-de/einen-speicherplatz-auf-einem-remote-server-erstellen-9B6F1BA0.html

## 1. Remote-Speicherplatz anlegen

In HBS 3 zunächst einen Remote-Speicherplatz anlegen. Je nach gewähltem Modus werden Hostname, Port, Benutzer, Authentifizierung und Zielpfad benötigt.

Den integrierten **Verbindungstest** verwenden. Falls die HBS-Version einen Speed-Test anbietet, den Wert dokumentieren; er ist später eine interessante Ergänzung zu unseren rclone-/SFTP-Messungen.

## 2. Backup statt bloßem Sync

Ein unidirektionaler Sync ist kein Ersatz für ein versioniertes Backup: Löschungen oder beschädigte Dateien können auf das Ziel repliziert werden.

Für wichtige Daten deshalb einen echten HBS-Backupjob mit geeigneter Versionierung/Retention bevorzugen, soweit die gewählte Zielart diese Funktionen unterstützt.

## 3. Verschlüsselung

Transportverschlüsselung und Backupverschlüsselung sind getrennte Eigenschaften.

Für sensible Daten sollte die Sicherung möglichst **vor dem Speichern auf HostBrr clientseitig verschlüsselt** werden. Wiederherstellungskennwörter/-schlüssel dürfen nicht ausschließlich auf dem QNAP liegen.

## 4. Zeitplan und Retention

Der Job sollte automatisch laufen und mindestens folgende Punkte festlegen:

- Backupintervall
- Aufbewahrungsregeln
- Verhalten bei Fehlern
- Benachrichtigung
- verfügbare Quota auf der StorageBox

Retention nicht durch manuelles Löschen unbekannter HBS-Metadaten auf dem Remote-Ziel ersetzen.

## 5. Verifikation

Nach der ersten Sicherung prüfen:

1. Jobstatus und Logs in HBS 3.
2. Plausible Datenmenge auf dem Ziel.
3. Fehler-/Warnmeldungen.
4. Einen echten Restore durchführen.

Ein erfolgreicher Upload allein beweist noch nicht, dass das Backup vollständig wiederherstellbar ist.

## 6. Restore-Test

Minimaltest:

1. Testordner mit mehreren Dateien sichern.
2. Eine Datei verändern und eine löschen.
3. Sicherung erneut ausführen.
4. Gewünschte Version in ein **separates Restore-Verzeichnis** zurückholen.
5. Dateinamen, Inhalte, Zeitstempel und – soweit relevant – Berechtigungen prüfen.

Für kritische NAS-Daten sollte gelegentlich ein größerer Restore-Test erfolgen.

## Sicherheit

- eigenes HostBrr-Zielverzeichnis für das NAS
- Backup-Credentials nicht für normale Administration wiederverwenden
- Schlüssel/Passwörter separat sichern
- StorageBox nicht als einzige Kopie wichtiger Daten behandeln
- bei Ransomware bedenken: ein dauerhaft beschreibbares Remote-Ziel ist nicht automatisch immutable

Siehe auch:

- [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md)
- [Ransomware & Löschschutz](../06-sicherheit/ransomware-loeschschutz.md)

## Typische Fehler

### Verbindungstest schlägt fehl

Prüfen:

- Hostname
- Port
- Benutzer
- Authentifizierung
- ob rsync auf der StorageBox verfügbar ist
- ob HBS den von HostBrr bereitgestellten rsync-/SSH-Modus unterstützt

### Job startet, bricht aber später ab

Dann zusätzlich Quota, Netzwerkabbrüche, Dateinamen, sehr große Dateien und HBS-Logs prüfen.

### Backup wächst unerwartet stark

Retention, Versionierung, Änderungsrate und temporäre/veränderliche Verzeichnisse prüfen. Nicht einfach remote Backupdateien löschen.

## Fortgeschrittene Alternative

Auf geeigneten QNAP-Modellen kann eine kontrollierte Container-/Linux-Umgebung mit Restic oder rclone flexibler sein. Das ist aber stark modell- und firmwareabhängig und deshalb kein allgemeiner Standardweg dieser Anleitung.

## HostBrr-spezifisch noch zu testen

Auf den realen Boxen prüfen wir später:

- welchen rsync-Modus HBS zuverlässig akzeptiert
- abweichenden SSH-Port
- Verhalten bei großen Dateien
- Verhalten bei vielen kleinen Dateien
- Resume nach Verbindungsabbruch
- Restore-Durchsatz
- Unterschiede zwischen 2-TB- und 8-TB-Box
