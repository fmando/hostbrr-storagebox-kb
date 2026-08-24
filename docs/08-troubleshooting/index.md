---
title: Troubleshooting
category: troubleshooting
status: maintained
last_reviewed: 2026-08-24
---
# Troubleshooting

Dieser Bereich sammelt konkrete Fehlerbilder mit reproduzierbaren Diagnose- und Lösungsschritten.

## Diagnoseprinzip

Vor Änderungen zuerst festhalten:

1. exakte Fehlermeldung
2. Datum/Uhrzeit
3. verwendetes Protokoll und Tool
4. Tool-Version
5. StorageBox-Standort/Generation soweit bekannt
6. ob der Fehler reproduzierbar ist
7. ob SSH/SFTP grundsätzlich funktioniert
8. aktuelle Quota bzw. freier Speicher

Danach erst Konfiguration verändern.

## Howtos

- [SSH & SFTP](ssh-sftp.md)
- [DirectAdmin, Quota & Cron](directadmin-quota-cron.md)
- [rclone, Restic, Borg & rsync](backup-tools.md)
- [Langsame Transfers & instabile Mounts](performance-mounts.md)
- [Nextcloud](nextcloud.md)

## Wichtig bei Shared Hosting

Viele allgemeine DirectAdmin-Anleitungen richten sich an Serveradministratoren mit Root-Zugriff. Auf einer HostBrr StorageBox haben wir diesen Zugriff nicht. Root-Kommandos aus der DirectAdmin-Dokumentation dienen daher teilweise nur dazu, die Ursache zu verstehen; serverseitige Reparaturen muss gegebenenfalls HostBrr durchführen.

## Supportfall vorbereiten

Ein guter Supportfall enthält mindestens:

```text
StorageBox/Tarif:
Standort:
Zeitpunkt:
Betroffener Dienst:
Client/OS:
Tool + Version:
Exakte Fehlermeldung:
Seit wann:
Reproduzierbar:
Bereits geprüft:
```

Passwörter, private SSH-Keys und Backup-Passphrases niemals in Tickets oder Screenshots offen mitsenden.
