---
title: Zuverlässigkeit, Wartung & Migrationen
status: researched
last_reviewed: 2026-08-24
---
# Zuverlässigkeit, Wartung & Migrationen

Dieser Bereich sammelt Erfahrungen zu Verfügbarkeit, Wartungen, Migrationen und dem Schutz der Daten auf der HostBrr StorageBox.

## RAID ist kein Backup

HostBrr beschreibt die aktuelle StorageBox-Plattform als RAID-60-HDD-Storage mit NVMe-Cache. RAID erhöht die Verfügbarkeit bei Laufwerksausfällen, ersetzt aber keine unabhängige Sicherung.

Offizielle Produktseite:

- https://hostbrr.com/storageboxes.html

Besonders wichtig ist eine Aussage des HostBrr-Vertreters `labze` im LowEndTalk-Thread vom Januar 2026: Auf die Frage, ob die StorageBoxen von HostBrr zusätzlich gesichert werden, lautet die Antwort **nein**; dies werde für das Produkt auch nicht behauptet. Im selben Thread wird die aktuelle Storage-Architektur als RAID-60 aus drei zugrunde liegenden RAID-6-Gruppen beschrieben.

Quelle:

- https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p22

### Konsequenz

Die StorageBox darf für wichtige Daten nicht die einzige Kopie sein.

```text
RAID-60 = Schutz gegen bestimmte Laufwerksausfälle
StorageBox = Offsite-Ziel möglich
StorageBox allein = keine vollständige Backupstrategie
```

## Community-Erfahrungen zur Verfügbarkeit

Ein Nutzer, der seine deutsche StorageBox permanent auf 7–10 Systemen per SSHFS eingebunden hat, erinnert sich für 2025 an drei Ausfälle: ungefähr 20 Minuten, rund eine Stunde und zwischen ein und zwei Stunden. Er beschreibt die Verfügbarkeit ansonsten als sehr gut.

Das ist eine einzelne Community-Erfahrung und keine SLA-Messung.

Quelle:

- https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p22

## Ungeplante Wartung

Im Dezember 2024 wurde im HostBrr-Statussystem ein ungeplanter Neustart eines deutschen Storage-Systems angekündigt, um kritische Performanceprobleme des HDD-RAID-Arrays zu beheben. Nutzer meldeten währenddessen unter anderem Ausfälle von ownCloud/Webdiensten, während DirectAdmin teilweise noch erreichbar war.

Quelle:

- https://lowendtalk.com/discussion/199617/hostbrr-bf-storage-deals-epyc-10-gbps-storage-vps-directadmin-storage-boxes-500gb-7-year/p31

Das zeigt, dass bei der Überwachung verschiedene Ebenen getrennt betrachtet werden sollten:

- DirectAdmin erreichbar?
- SSH/SFTP erreichbar?
- Website/Nextcloud erreichbar?
- Dateioperationen erfolgreich?

## Migrationen können lange dauern

Bei einer US-StorageBox-Migration im Januar 2026 wurden Accounts während der Migration deaktiviert. HostBrr hatte laut zitierter Kundenmail mit ungefähr 4–12 Stunden pro Batch gerechnet. Einige Nutzer meldeten jedoch 36 Stunden bis drei Tage Downtime.

HostBrr erklärte die Verzögerungen insbesondere mit Accounts, die sehr große Mengen kleiner Dateien enthielten. Für große Dateien seien wesentlich höhere Transferraten möglich gewesen als für Millionen kleiner Dateien.

Quelle:

- https://lowendtalk.com/discussion/213630/hostbrr-us-storagebox-migration

### Lehre für unsere KB

Die Dateistruktur beeinflusst nicht nur den normalen Transfer, sondern auch Wartung und Provider-Migrationen.

Millionen kleiner Dateien können eine Migration erheblich verlängern.

## Hostname und IP können sich ändern

Bei der genannten US-Migration wurden neuer Hostname und neue IP angekündigt. Automatisierte Backups sollten deshalb nicht unnötig hart auf eine IP-Adresse verdrahtet werden.

Empfehlung:

- Hostname statt IP verwenden, sofern möglich
- SSH-Host-Key-Änderungen nach einer angekündigten Migration bewusst prüfen
- DNS nicht blind als Beweis für Identität betrachten
- Monitoring nach Migration kontrollieren

## Statusseite

HostBrr betreibt eine öffentliche Statusseite:

- https://status.hostbrr.com/

Bei Problemen sollte sie neben dem eigenen Monitoring geprüft werden.

## Monitoring-Empfehlung

Für eine StorageBox als Backupziel sind mindestens folgende Prüfungen sinnvoll:

1. SSH/SFTP-Verbindung möglich
2. Testdatei lesbar
3. freier Speicher/Quota ausreichend
4. letztes Backup erfolgreich
5. Alter des letzten Snapshots
6. regelmäßiger Restore-Test

Nur `ping` oder eine erreichbare Webseite beweist nicht, dass das Backupziel korrekt funktioniert.

## Bewertung

| Aspekt | Aktueller Wissensstand |
|---|---|
| RAID-Schutz | offiziell/community-bestätigt |
| zusätzliche HostBrr-Backups | laut Providervertreter: nein |
| längere Migrationen möglich | community + Providerantwort |
| kleine Dateien problematisch | mehrfach community/provider-bestätigt |
| SLA/Uptime-Garantie speziell StorageBox | noch zu klären |
| unabhängige Langzeit-Uptime-Messung | noch auszubauen |

## Offene Punkte

- StorageBox-spezifische SLA-/ToS-Regeln prüfen
- historische Statusmeldungen systematisch archivieren
- eigene Verfügbarkeitsmessung für 2-TB- und 8-TB-Box aufsetzen
- Verhalten bei Quota-Vollstand und RAID-Wartung praktisch untersuchen
