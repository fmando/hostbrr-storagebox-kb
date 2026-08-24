---
title: BorgBackup
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# BorgBackup mit der HostBrr StorageBox

[BorgBackup](https://www.borgbackup.org/) ist ein deduplizierendes Backup-Programm mit Kompression und authentifizierter Verschlüsselung.

## HostBrr-spezifischer Stand

Aus Community-Berichten wissen wir, dass Borg auf mindestens einer früheren HostBrr-StorageBox-Generation serverseitig vorhanden war. Das bedeutet **nicht**, dass Borg auf jeder aktuellen StorageBox oder an jedem Standort installiert ist.

Vor der Einrichtung deshalb Client und Server prüfen:

```bash
borg --version
ssh -p <PORT> <USER>@<HOST> 'borg --version'
```

Borg 1 und Borg 2 unterscheiden sich bei Befehlen und Repositoryformaten erheblich. Beispiele aus der Dokumentation müssen daher immer zur tatsächlich installierten Hauptversion passen.

## Architektur

```text
Quellsystem
  ↓ Borg Client
SSH
  ↓ borg serve
HostBrr StorageBox
  ↓
verschlüsseltes Borg Repository
```

Für effiziente Remote-Repositories startet Borg auf dem Ziel normalerweise `borg serve`. Genau deshalb ist die serverseitig installierte Borg-Version für HostBrr wichtiger als bei Restic oder Kopia über SFTP.

## Voraussetzungen

- funktionierender SSH-Zugang
- tatsächlicher HostBrr-SSH-Port
- Borg auf Client und StorageBox
- kompatible Borg-Versionen
- ausreichend freie Quota
- sicherer Ort für Passphrase und Repository-Key

## Repository anlegen

Das konkrete Kommando hängt von der Borg-Hauptversion ab. Für eine aktuelle Installation zuerst die zur installierten Version passende offizielle Dokumentation verwenden:

- https://borgbackup.readthedocs.io/

Repository-URLs über SSH folgen grundsätzlich dem Borg-Remote-Modell. Den Zielpfad zunächst mit einem Testrepository verifizieren.

## Backup

Ein typischer Borg-Ablauf besteht aus:

1. Repository öffnen/erstellen.
2. Archiv erzeugen.
3. Exitcode und Log prüfen.
4. Retention anwenden.
5. Repository regelmäßig prüfen.
6. Restore testen.

Zu sichernde Systemdaten benötigen gegebenenfalls Root-Rechte auf dem Client. Borg empfiehlt konsistente Benutzerrechte für Repositoryzugriffe.

## Verschlüsselung

Borg unterstützt authentifizierte Verschlüsselung. Passphrase und Repository-Key gehören **nicht nur auf das zu sichernde System**.

Für Disaster Recovery separat dokumentieren:

```text
Repository:
Borg-Version:
Verschlüsselungsmodus:
Key-Backup liegt wo?:
Passphrase liegt wo?:
Letzter Restore-Test:
```

## Retention: prune und compact

Retention ist versionsabhängig zu konfigurieren. Bei Borg 2 markiert `borg prune` nicht mehr benötigte Archive zunächst zur Löschung; freier Speicher entsteht erst durch `borg compact`.

Vor produktivem Löschen immer zuerst einen Dry Run durchführen. Bei mehreren Backupserien im selben Repository muss die Auswahl der Archive ausdrücklich eingeschränkt werden, damit nicht versehentlich fremde Serien in die Retention einbezogen werden.

## Integritätsprüfung

Borg besitzt mit `borg check` eine eigene Repository- und Archivprüfung. Je nach Version kann zusätzlich eine vollständige Datenverifikation aktiviert werden; diese ist deutlich aufwendiger.

Bei einem Remote-Repository können Repositoryprüfungen serverseitig laufen, während Prüfungen verschlüsselter Archivinhalte teilweise den Client benötigen.

**`--repair` nicht als normalen Wartungsbefehl verwenden.** Reparatur verändert das Repository und gehört nur in einen bewusst geplanten Recovery-Fall.

## Quota und Reserve

Ein volles Repository ist besonders unangenehm, weil auch Aufräumoperationen freien Platz benötigen können. Borg 2 bietet dafür `borg repo-space`, mit dem Notfallplatz reserviert werden kann. Ob und wie sinnvoll das auf der HostBrr-Quota funktioniert, testen wir später praktisch.

Unabhängig davon sollte die StorageBox nicht bis auf die letzten Gigabyte gefüllt werden.

## Restore

Ein Restore-Test gehört zwingend zur Einrichtung:

1. Archivliste anzeigen.
2. einen bekannten Snapshot auswählen.
3. einzelne Datei in ein leeres Testziel extrahieren.
4. kompletten Testordner extrahieren.
5. Inhalt, Rechte und Zeitstempel prüfen.

Ein kompletter Disaster-Recovery-Test sollte zusätzlich sicherstellen, dass Key und Passphrase auf einem frischen System verfügbar sind.

## Automatisierung

Für systemd/Cron:

- keine Passphrase im Klartext in der Kommandozeile,
- Exitcodes auswerten,
- Logs aufbewahren,
- Backup, Retention und Prüfung nicht unkontrolliert parallel starten,
- regelmäßig freien StorageBox-Platz prüfen.

## Typische Fehler

### `borg: command not found` auf HostBrr

Borg ist auf diesem Node nicht installiert oder nicht im PATH. Dann ist diese Remote-Variante nicht verfügbar. Restic/Kopia über SFTP sind weniger abhängig von serverseitiger Software.

### Versions-/Repositoryfehler

Client- und Serverversion dokumentieren. Keine Borg-1-Anleitung blind mit Borg 2 verwenden.

### Repository voll

Nicht sofort destruktiv löschen. Quota prüfen und einen kontrollierten Retention-/Compact-Lauf planen.

### Backup läuft, Restore nicht

Key/Passphrase und Restorepfad auf einem separaten Testsystem prüfen.

## HostBrr-spezifische offene Tests

- Borg auf aktueller 2-TB-Box vorhanden?
- Borg auf aktueller 8-TB-Box vorhanden?
- genaue Client-/Serverversionen
- erlaubte Remote-Kommandos
- Repositorycheck unter Shared-Hosting-Limits
- prune/compact und Quota-Verhalten
- große Datei vs. viele kleine Dateien
- vollständiger Restore auf frischem System

Bis diese Punkte selbst geprüft wurden, bleibt der Artikel `community-reported`.

## Primärquellen

- BorgBackup: https://www.borgbackup.org/
- Dokumentation: https://borgbackup.readthedocs.io/
- Check: https://borgbackup.readthedocs.io/en/latest/usage/check.html
- Prune: https://borgbackup.readthedocs.io/en/latest/usage/prune.html
- Quick Start: https://borgbackup.readthedocs.io/en/latest/quickstart.html
