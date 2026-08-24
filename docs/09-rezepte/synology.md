---
title: Synology NAS auf HostBrr sichern
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Synology NAS auf HostBrr sichern

Für Synology ist **Hyper Backup über rsync im Remote-Shell-Modus** der naheliegende native Ansatz, sofern die konkrete HostBrr-StorageBox den benötigten rsync-over-SSH-Zugriff zulässt.

## Zielbild

```text
Synology NAS
  ↓ Hyper Backup / rsync Remote Shell
SSH
  ↓
HostBrr StorageBox
```

## Warum Remote Shell?

Synology unterscheidet bei rsync-Zielen zwischen rsync-Daemon und Remote-Shell-Modus. Der Remote-Shell-Modus wird verwendet, wenn ein absoluter Zielpfad angegeben wird und transportiert über eine verschlüsselte Verbindung.

Offizielle Dokumentation:

- Synology Hyper Backup – Ziele: https://kb.synology.com/de-de/DSM/help/HyperBackup/data_backup_destination

## HostBrr-Voraussetzungen

Vorher prüfen:

- Hostname
- SSH-Port
- Benutzername
- Zielpfad
- rsync-Verfügbarkeit
- Quota

Auf der StorageBox:

```bash
rsync --version
pwd
```

## Backup-Modus

Hyper Backup kann bei rsync-Zielen mehrere Versionen oder eine einzelne Version verwenden. Bei mehreren Versionen speichert Synology die Sicherung im eigenen `.hbk`-Format und bietet Verschlüsselung und Komprimierung. Eine Einzelversion bleibt direkt als normale Dateien lesbar, bietet in diesem Modus aber nicht dieselben Verschlüsselungs-/Kompressionsoptionen.

Für ein echtes Backup ist die Mehrversionsvariante meist interessanter.

## Verschlüsselung

Zwischen zwei Begriffen unterscheiden:

- **Transportverschlüsselung** schützt die Übertragung.
- **Backupverschlüsselung** schützt die auf HostBrr gespeicherten Daten.

Für sensible NAS-Daten sollte die Sicherung bereits clientseitig auf der Synology verschlüsselt werden.

## Restore

Nicht nur den Backupjob anlegen. In Hyper Backup sollte anschließend testweise mindestens ein Verzeichnis zurückgesichert werden. Zusätzlich muss der Wiederherstellungsschlüssel bzw. das Verschlüsselungspasswort außerhalb des NAS sicher verwahrt werden.

## Noch zu verifizieren

Auf einer realen HostBrr-Box prüfen wir später insbesondere, welche Kombination aus Synology Hyper Backup, HostBrr-SSH-Port und Remote-Shell-rsync ohne Sonderkonfiguration funktioniert.
