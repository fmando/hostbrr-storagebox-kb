---
title: Synology NAS auf HostBrr sichern
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Synology NAS auf HostBrr sichern

## Ziel

Ein Synology NAS soll automatisiert, versioniert und möglichst clientseitig verschlüsselt auf eine HostBrr StorageBox sichern.

## Architektur

```text
Synology NAS
  ↓ Hyper Backup
rsync Remote Shell / SSH
  ↓
HostBrr StorageBox
```

Der native Kandidat ist **Hyper Backup mit einem rsync-kompatiblen Ziel im Remote-Shell-Modus**, sofern die konkrete HostBrr-Box diese Kombination unterstützt.

## Voraussetzungen

Vorher dokumentieren:

- HostBrr-Hostname
- SSH-Port
- Benutzername
- tatsächlicher Zielpfad
- freie Quota
- rsync-Verfügbarkeit
- DSM-/Hyper-Backup-Version

Auf der StorageBox prüfen:

```bash
rsync --version
pwd
```

Siehe auch:

- [SSH & SFTP](../02-zugang/ssh-sftp.md)
- [File Manager & Pfade](../02-zugang/directadmin-filemanager-pfade.md)
- [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md)

## Hyper Backup konfigurieren

Synology unterscheidet bei rsync-Zielen zwischen rsync-Daemon und Remote-Shell-Modus. Für den Remote-Shell-Modus wird ein absoluter Zielpfad verwendet.

Offizielle Dokumentation:

- https://kb.synology.com/de-de/DSM/help/HyperBackup/data_backup_destination
- https://kb.synology.com/de-de/DSM/help/HyperBackup/data_backup_settings

Für HostBrr müssen Host, Port, Benutzer und Zielpfad aus der eigenen Box übernommen werden. Keine Beispielpfade blind kopieren.

## Mehrere Versionen oder Einzelversion?

Für ein echtes Backup ist normalerweise **Mehrere Versionen** interessanter. Synology speichert diese Sicherung als `.hbk`, unterstützt dabei Komprimierung und clientseitige Verschlüsselung und kann Versionen rotieren.

Eine Einzelversion bleibt direkt als normale Dateien lesbar, überschreibt aber ältere Sicherungsstände und bietet bei diesem rsync-Modus nicht dieselben Verschlüsselungs-/Kompressionsmöglichkeiten.

## Verschlüsselung

Zwei Ebenen unterscheiden:

- Transportverschlüsselung schützt die Verbindung.
- Clientseitige Backupverschlüsselung schützt die auf HostBrr gespeicherten Sicherungsdaten.

Bei Hyper Backup kann clientseitige Verschlüsselung beim Erstellen der Aufgabe aktiviert werden. **Passwort und heruntergeladener Verschlüsselungsschlüssel müssen separat sicher verwahrt werden.** Ein Verlust kann die Wiederherstellung unmöglich machen.

## Automatisierung und Rotation

In Hyper Backup einen Backup-Zeitplan und eine passende Versionsrotation definieren. Die Aufbewahrung sollte zur Datenänderungsrate und verfügbaren Quota passen.

Benachrichtigungen für fehlgeschlagene Jobs aktivieren. Ein still fehlgeschlagenes NAS-Backup ist besonders gefährlich, weil es oft erst im Restore-Fall auffällt.

## Verifikation

Synology bietet eine eigene **Integritätsprüfung**. Dabei wird die Indexstruktur geprüft; optional können auch die Backupdaten geprüft werden. Diese Prüfung sollte regelmäßig geplant werden.

Wichtig: `.hbk`-Inhalte nicht direkt auf der StorageBox verändern oder löschen. Synology warnt, dass direkte Änderungen am Sicherungsziel das Backup beschädigen können.

## Restore-Test

Nach der Einrichtung nicht bei „Job erfolgreich“ aufhören:

1. kleinen Testordner sichern,
2. eine Datei lokal verändern oder entfernen,
3. eine ältere Version in einen separaten Restore-Ordner zurückholen,
4. Inhalt, Dateinamen und Zeitstempel prüfen,
5. dokumentieren, wo Passwort und Verschlüsselungsschlüssel liegen.

Für größere Tests kann Hyper Backup Explorer als zusätzlicher Recovery-Weg relevant sein.

## Typische Fehler

### Verbindung schlägt fehl

Prüfen:

- Hostname und Port
- Benutzer/Authentifizierung
- Zielpfad
- ob `rsync --version` auf der Box funktioniert
- ob der verwendete Hyper-Backup-Modus wirklich Remote Shell erwartet

### Backup läuft, ist aber sehr langsam

Viele kleine Dateien verursachen wesentlich mehr Metadatenoperationen als große Archive. Siehe [Große vs. kleine Dateien](../07-performance/grosse-kleine-dateien.md).

### Quota läuft voll

Versionierung benötigt zusätzlichen Speicher. Rotation kontrollieren und nicht einfach Dateien innerhalb eines `.hbk` manuell löschen.

## Sicherheit

Die StorageBox ist Offsite-Speicher, aber kein garantiert unveränderliches Ziel. Ein kompromittierter Backupzugang kann Sicherungen gefährden. Für wichtige Daten deshalb weitere Kopie/Generation nach der [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md) vorsehen.

## HostBrr-spezifisch noch zu verifizieren

Auf den realen Boxen prüfen wir später:

- rsync Remote Shell aus Hyper Backup
- HostBrr-SSH-Port und Pfadbehandlung
- Mehrversionsbackup + clientseitige Verschlüsselung
- Integritätsprüfung über ein großes Repository
- Restore-Geschwindigkeit
- Verhalten 2 TB vs. 8 TB
