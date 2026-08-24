---
title: Große Datenmengen erstmals übertragen
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Große Datenmengen erstmals auf die StorageBox übertragen

Der initiale Upload mehrerer Terabyte ist ein anderer Anwendungsfall als das tägliche inkrementelle Backup.

## Vorher rechnen

Theoretische Mindestdauer ohne Overhead:

| effektiver Durchsatz | 1 TB | 2 TB | 8 TB |
|---:|---:|---:|---:|
| 10 MB/s | ~28 h | ~56 h | ~9,3 Tage |
| 50 MB/s | ~5,6 h | ~11 h | ~1,9 Tage |
| 100 MB/s | ~2,8 h | ~5,6 h | ~22 h |

Reale Zeiten können durch kleine Dateien, Latenz, Verschlüsselung, Routing und Limits deutlich höher liegen.

## Empfehlung: rclone statt Browser/File Manager

Für große Transfers ist ein wiederaufnehmbares CLI-Werkzeug sinnvoller.

Offizielle Dokumentation: https://rclone.org/sftp/

Beispiel:

```bash
rclone copy /daten hostbrr:/initial/daten \
  --progress \
  --transfers 4 \
  --checkers 8
```

Die Parallelität sollte nicht blind maximiert werden. Shared Hosting, SFTP-CPU-Last, Quellstorage und Netzwerk können jeweils zum Flaschenhals werden.

## Viele kleine Dateien

Millionen kleiner Dateien verursachen deutlich mehr Metadaten- und Protokolloperationen als wenige große Archive.

Je nach Anwendungsfall kann es sinnvoll sein, unveränderliche Dateibäume zunächst zu archivieren:

```bash
tar -cf archiv.tar /pfad/daten
```

Bei Backups sollte jedoch nicht unnötig jedes Mal ein riesiges Vollarchiv erzeugt werden; Restic/Borg lösen inkrementelle und deduplizierte Sicherungen eleganter.

## Prüfen statt vertrauen

Nach dem initialen Transfer sollten mindestens Dateizahl und Datenmenge verglichen werden. Für besonders wichtige unveränderliche Daten sind Checksums sinnvoll.

rclone kann je nach Backend und unterstützten Hashes prüfen:

```bash
rclone check /daten hostbrr:/initial/daten
```

Bei SFTP stehen nicht zwingend dieselben serverseitigen Hash-Funktionen wie bei Object Storage zur Verfügung; Verhalten und Performance müssen praktisch geprüft werden.

## Relay-VPS

Wenn der direkte Pfad vom eigenen Internetanschluss zur StorageBox schlecht geroutet ist, kann ein gut angebundener VPS als Zwischenstation helfen. Das ist jedoch ein zusätzlicher Verarbeitungsschritt und sollte nur eingesetzt werden, wenn Messungen einen echten Vorteil zeigen.

## Transferkontingent beachten

Vor mehrtägigen oder mehrfach wiederholten Multi-TB-Transfers sollte das im eigenen HostBrr-Tarif enthaltene Transferkontingent geprüft werden.
