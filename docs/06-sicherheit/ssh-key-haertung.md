---
title: SSH-Key-Härtung
category: sicherheit
status: maintained
last_reviewed: 2026-08-24
---
# SSH-Key-Härtung

SSH-Keys sind für automatisierte Backups ideal, sollten aber möglichst wenig Rechte besitzen.

## Empfehlungen

- für Backups einen **eigenen Schlüssel** erzeugen
- keinen privaten Schlüssel zwischen mehreren unabhängigen Systemen wiederverwenden
- Ed25519 verwenden, sofern Client und Server es unterstützen
- private Schlüssel lokal mit restriktiven Dateirechten speichern
- wenn möglich einen Schlüssel nur für den jeweiligen Backup-Zweck verwenden
- veraltete bzw. nicht mehr benötigte Keys regelmäßig aus DirectAdmin entfernen

Beispiel:

```bash
ssh-keygen -t ed25519 -a 64 -f ~/.ssh/hostbrr-backup
chmod 600 ~/.ssh/hostbrr-backup
```

## Automatisierung

Bei unbeaufsichtigten Backups ist eine Passphrase auf dem privaten Schlüssel nur dann praktikabel, wenn ein sicherer Agent oder Secret-Mechanismus die Freigabe übernimmt. Ein unverschlüsselter Automations-Key erhöht dagegen die Bedeutung von Dateirechten und Least Privilege.

## Host-Key-Prüfung niemals leichtfertig abschalten

Optionen wie

```text
StrictHostKeyChecking=no
```

sind bequem, schwächen aber die Prüfung der Serveridentität. Besser ist, den erwarteten Host-Key kontrolliert in `known_hosts` aufzunehmen und Änderungen zu untersuchen.

## DirectAdmin

Je nach HostBrr-Konfiguration können Public Keys über DirectAdmin verwaltet werden. Die tatsächlich sichtbaren Menüs werden wir später auf einer aktuellen StorageBox dokumentieren.

Offizielle DirectAdmin-Dokumentation:

- https://docs.directadmin.com/operation-system-level/os-general/managing-with-ssh.html

## Zielbild

Für Backups sollte ein kompromittierter Quellserver möglichst **nicht automatisch Vollzugriff auf andere Sicherungen** erhalten. Wo HostBrr oder die eingesetzten Tools Einschränkungen per Pfad oder Command zulassen, sollten wir diese nutzen und später konkret testen.
