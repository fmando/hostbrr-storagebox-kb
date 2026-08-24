---
title: Bedrohungsmodell
category: sicherheit
status: maintained
last_reviewed: 2026-08-24
---
# Bedrohungsmodell für die HostBrr StorageBox

Eine gute Backup-Strategie beginnt mit der Frage, **wogegen** sie schützen soll. Eine StorageBox schützt nicht automatisch vor jedem Szenario.

## Relevante Risiken

- versehentliches Löschen
- fehlerhafte Synchronisation
- Ransomware auf dem Quellsystem
- kompromittierte SSH-/SFTP-Zugangsdaten
- kompromittierter DirectAdmin-Account
- Ausfall oder Verlust des HostBrr-Accounts
- Speicher- oder Dateisystemfehler
- Verlust von Verschlüsselungsschlüsseln
- Diebstahl vertraulicher Daten aus dem Backup

## Grundprinzip

Die StorageBox sollte **eine Offsite-Kopie** sein, nicht die einzige Sicherung. Für besonders wichtige Daten ist zusätzlich eine zweite, logisch getrennte Kopie sinnvoll, die von einem kompromittierten Quellsystem nicht direkt gelöscht werden kann.

CISA empfiehlt für Ransomware-Schutz ausdrücklich offline bzw. anderweitig nicht direkt erreichbare, verschlüsselte Backups und regelmäßige Restore-Tests. Außerdem werden Immutable-/Delete-Protection und Versionierung empfohlen, sofern das Speichersystem sie unterstützt.

## Konsequenz für HostBrr

Solange für die StorageBox keine echte serverseitige Immutability oder Object-Lock-Funktion verifiziert ist, sollten wir davon ausgehen, dass ein Angreifer mit gültigen Zugangsdaten auch Backups löschen kann.

Daher sind besonders wichtig:

1. clientseitige Verschlüsselung
2. getrennte Zugangsdaten
3. möglichst eingeschränkte SSH-Keys
4. versionierte Backups
5. zusätzliche Kopie außerhalb des normalen Backup-Pfads
6. regelmäßige Restore-Tests

## Weiterführende Dokumentation

- CISA #StopRansomware Guide: https://www.cisa.gov/stopransomware/ransomware-guide
- CISA Backup-/Ransomware-Empfehlungen: https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-165a
