---
title: BorgBackup
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# BorgBackup mit der HostBrr StorageBox

[BorgBackup](https://www.borgbackup.org/) ist ein deduplizierendes Backup-Programm mit Kompression und authentifizierter Verschlüsselung.

## HostBrr-spezifischer Stand

Aus Community-Berichten wissen wir, dass Borg auf mindestens einer früheren HostBrr-StorageBox-Generation serverseitig vorhanden war. Das bedeutet jedoch **nicht**, dass Borg auf jeder aktuellen StorageBox oder an jedem Standort installiert ist.

Vor der Einrichtung daher zuerst prüfen:

```bash
ssh -p <PORT> <USER>@<HOST> 'borg --version'
```

Der tatsächliche SSH-Port muss aus den Zugangsdaten der eigenen StorageBox übernommen werden.

## Warum serverseitiges Borg wichtig ist

Ein Borg-Repository über SSH verwendet auf dem Ziel normalerweise `borg serve`. Dadurch kann Borg effizient mit dem entfernten Repository arbeiten. Die offizielle Dokumentation beschreibt Remote-Repositories und `borg serve` ausführlich:

- [Borg: Remote repositories](https://borgbackup.readthedocs.io/en/stable/usage/general.html#repository-urls)
- [Borg: borg serve](https://borgbackup.readthedocs.io/en/stable/usage/serve.html)

## Repository initialisieren

Wenn Borg serverseitig verfügbar ist, sieht ein Repository beispielsweise so aus:

```bash
borg init --encryption=repokey-blake2 \
  ssh://<USER>@<HOST>:<PORT>/./backups/borg
```

Danach kann ein Backup beispielsweise mit `borg create` erzeugt werden.

## Sicherheit

Bei verschlüsselten Borg-Repositories liegen die Nutzdaten verschlüsselt auf der StorageBox. Entscheidend ist, dass Passphrase und Repository-Key unabhängig vom Backupziel gesichert werden.

## Noch zu verifizieren

- Borg auf aktueller 2026er StorageBox vorhanden?
- installierte Borg-Version je Standort/Generation
- erlaubte Kommandos über SSH
- Performance bei großen und vielen kleinen Dateien
- Restore auf ein frisches System

Bis diese Punkte selbst geprüft wurden, bleibt der Artikel `community-reported`.

## Weiterführende Dokumentation

- [BorgBackup](https://www.borgbackup.org/)
- [Borg Documentation](https://borgbackup.readthedocs.io/)
- [Borg Repository URLs](https://borgbackup.readthedocs.io/en/stable/usage/general.html#repository-urls)
- [borg serve](https://borgbackup.readthedocs.io/en/stable/usage/serve.html)
