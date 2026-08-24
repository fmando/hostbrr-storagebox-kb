---
title: "HostBrr Storage Boxes – rsync backup example"
source_type: forum
source_name: LowEndTalk
url: "https://lowendtalk.com/discussion/205325/hostbrr-storage-boxes-any-experiences-with-them-good-or-bad"
published: 2025-05
retrieved: 2026-08-24
reliability: community
storagebox_generation: 2025
---

# rsync-Backup per DirectAdmin-Cron

Ein Nutzer beschreibt einen praktisch eingesetzten Pull-Backup-Aufbau:

1. SSH-Key liegt im Home-Verzeichnis der StorageBox.
2. Ein DirectAdmin-Cronjob läuft auf der StorageBox.
3. `rsync` verbindet sich von dort per SSH zum zu sichernden VPS.
4. Ausgeschlossene Verzeichnisse wie Python-`venv` und `__pycache__` werden nicht übertragen.
5. Ziel ist ein Backup-Verzeichnis im StorageBox-Home.

## Bedeutung

Damit ist zumindest für die damalige Generation belegt, dass rsync und Cronjobs gemeinsam für automatisierte Pull-Backups genutzt wurden.

## Sicherheitsverbesserungen für unser Howto

- dedizierter SSH-Key nur für Backup
- private Key-Datei mit restriktiven Rechten
- eingeschränkter Backup-Benutzer auf dem Quellserver
- Verschlüsselung der Backup-Inhalte prüfen
- Restore-Test dokumentieren

Der Community-Befehl wird nicht ungeprüft kopiert; wir erstellen einen eigenen, gehärteten Ablauf.
