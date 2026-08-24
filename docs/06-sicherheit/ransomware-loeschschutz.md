---
title: Schutz vor Ransomware und Löschungen
category: sicherheit
status: maintained
last_reviewed: 2026-08-24
---
# Schutz vor Ransomware und Löschungen

Offsite allein reicht nicht. Ein Backupziel, das mit denselben dauerhaft verfügbaren Credentials schreibbar ist, kann bei einer Kompromittierung ebenfalls betroffen sein.

## Angriffswege

- Malware liest gespeicherte SFTP-/SSH-Zugangsdaten aus.
- Ein Backup-Script wird manipuliert und löscht alte Sicherungen.
- Ein Benutzerfehler mit `rclone sync --delete-*` oder `rsync --delete` repliziert Löschungen.
- Ein kompromittierter DirectAdmin-Account löscht Dateien oder verändert SSH-Keys.

## Maßnahmen

### 1. Versionierte Backupsoftware verwenden

Restic und Borg speichern Snapshots/Archive statt nur einen aktuellen Spiegelzustand. Das reduziert das Risiko, dass eine einzelne fehlerhafte Synchronisation alle älteren Zustände ersetzt.

### 2. Retention bewusst konfigurieren

Retention sollte nicht im selben unkontrollierten Schritt erfolgen wie das eigentliche Backup. Erst Backup erfolgreich abschließen, dann nach klarer Policy alte Stände entfernen.

### 3. Separate Credentials

Backup-Zugangsdaten nicht für interaktive Alltagszugriffe wiederverwenden. Wo möglich: eigener Key, eigener Pfad, eingeschränkte Berechtigung.

### 4. Eine nicht dauerhaft schreibbare Kopie vorhalten

CISA empfiehlt offline bzw. immutable Backups, weil viele Ransomware-Varianten erreichbare Sicherungen gezielt löschen oder verschlüsseln.

### 5. DirectAdmin absichern

- starkes, einzigartiges Passwort
- 2FA aktivieren, sofern verfügbar
- unbekannte SSH-Keys entfernen
- nicht benötigte Zugänge löschen

### 6. Restore statt nur Backup testen

Ein `backup successful` beweist noch nicht, dass die Daten vollständig und praktisch wiederherstellbar sind.

## HostBrr-spezifischer offener Punkt

Ob die aktuelle StorageBox serverseitige Funktionen wie Snapshots, WORM, Object Lock oder eine vergleichbare Löschsperre bietet, ist derzeit nicht verifiziert. Bis dahin behandeln wir die per SSH/SFTP erreichbaren Daten als **löschbar durch jeden Account mit ausreichenden Rechten**.

## Weiterführende Dokumentation

- CISA #StopRansomware Guide: https://www.cisa.gov/stopransomware/ransomware-guide
- Restic Retention: https://restic.readthedocs.io/en/stable/060_forget.html
- Borg Prune: https://borgbackup.readthedocs.io/en/stable/usage/prune.html
