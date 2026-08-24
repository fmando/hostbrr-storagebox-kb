---
title: 2 TB vs. 8 TB StorageBox
category: grundlagen
status: planned-verification
last_reviewed: 2026-08-24
---
# 2 TB vs. 8 TB StorageBox

Diese Seite bereitet einen späteren direkten Vergleich einer HostBrr StorageBox mit 2 TB und einer mit 8 TB vor.

## Offizielle Gemeinsamkeiten

Nach der aktuellen HostBrr-Produktbeschreibung gehören beide Größen zur regulären DirectAdmin-StorageBox-Serie in Frankfurt. Für alle Größen nennt HostBrr dieselbe RAID-60-Plattform mit NVMe-Cache sowie 10-Gbit/s-Connectivity und SSH/FTP/rSync-Zugriff.

| Merkmal | 2 TB | 8 TB |
|---|---:|---:|
| Speicher | 2 TB | 8 TB |
| Transfer | 10 TB | 40 TB |
| nominelle Connectivity | 10 Gbit/s | 10 Gbit/s |
| Standort | Frankfurt | Frankfurt |
| Panel | DirectAdmin | DirectAdmin |
| SSH/FTP/rSync | ja | ja |
| Softaculous | ja | ja |

Quellen:

- https://my.hostbrr.com/order/main/packages/storagebox/?group_id=63
- https://hostbrr.com/storageboxes.html

## Die zentrale Frage

HostBrr schreibt, dass jede Größe auf derselben redundanten RAID-60-Plattform läuft. Daraus folgt jedoch **nicht automatisch**, dass 2-TB- und 8-TB-Accounts dieselben CPU-, RAM-, I/O-, IOPS-, Prozess- oder Parallelitätslimits besitzen.

Diese Werte werden auf der öffentlichen StorageBox-Produktseite derzeit nicht vollständig ausgewiesen.

Deshalb werden wir Produktdaten und gemessene Accountdaten strikt trennen.

## Inventarisierung vor Performance-Tests

Bevor Benchmarks durchgeführt werden, erfassen wir auf beiden Accounts möglichst identisch:

- Bestell-/Produktbezeichnung
- Bereitstellungsdatum
- Hostname
- Standort
- DirectAdmin-Version
- sichtbare Account-/Package-Limits
- Disk-Quota
- Bandwidth-Quota
- CPU-/RAM-/LVE-Anzeigen, sofern sichtbar
- I/O-/IOPS-Limits, sofern sichtbar
- Prozess-/Entry-Process-Limits, sofern sichtbar
- SSH-Port
- Shell
- PHP-Versionen
- Datenbankversion
- verfügbare Kommandozeilenprogramme
- Borg-Version, falls vorhanden
- rsync-Version
- SSH-Version
- Dateisysteminformationen, soweit für den Benutzer sichtbar

Secrets, IP-Adressen oder andere sensible Accountdaten werden nicht veröffentlicht, sofern sie für die technische Aussage nicht erforderlich sind.

## Vergleich der verfügbaren Programme

Auf beiden Boxen sollen später mindestens folgende Befehle geprüft werden:

```bash
command -v rsync
command -v borg
command -v restic
command -v rclone
command -v ssh
command -v sftp
command -v php
command -v python3

rsync --version
borg --version
ssh -V
php -v
python3 --version
```

Fehlende Programme sind ebenfalls ein relevantes Ergebnis.

## Vergleich der DirectAdmin-Accounts

Von besonderem Interesse ist, ob DirectAdmin bzw. CloudLinux bei beiden Tarifen unterschiedliche Ressourcenlimits anzeigt.

Zu erfassen sind beispielsweise:

```text
2 TB
CPU:
RAM:
I/O:
IOPS:
Processes:
Entry Processes:
Disk quota:
Bandwidth:

8 TB
CPU:
RAM:
I/O:
IOPS:
Processes:
Entry Processes:
Disk quota:
Bandwidth:
```

## Spätere Performance-Matrix

Erst nach der Inventarisierung folgt der eigentliche Benchmark.

| Test | 2 TB | 8 TB |
|---|---:|---:|
| großer Upload | offen | offen |
| großer Download | offen | offen |
| viele kleine Dateien | offen | offen |
| rsync | offen | offen |
| rclone 1 Transfer | offen | offen |
| rclone 4 Transfers | offen | offen |
| rclone 8 Transfers | offen | offen |
| Metadatenoperationen | offen | offen |
| SSH-Latenz | offen | offen |
| Restore | offen | offen |

## Testbedingungen

Damit der Vergleich aussagekräftig ist, müssen beide Boxen möglichst vom **gleichen Quellsystem**, zur **gleichen Tageszeit bzw. unmittelbar nacheinander**, mit denselben Dateien und identischen Programmparametern getestet werden.

Andernfalls könnten Routing, Quellbandbreite, Tageszeit oder Clienthardware fälschlich wie ein Tarifunterschied aussehen.

## Hypothesen

Vor den Tests halten wir mehrere Möglichkeiten offen:

1. Beide Tarife unterscheiden sich nur in Speicher und Transferkontingent.
2. Größere Tarife erhalten höhere Account-/LVE-Limits.
3. Beide Accounts haben identische Limits, liegen aber auf unterschiedlichen Storage-Nodes und zeigen deshalb unterschiedliche Performance.
4. Unterschiede entstehen primär durch aktuelle Node-Auslastung und nicht durch den Tarif.

Keine dieser Hypothesen wird vor der Messung als Fakt behandelt.

## Abgrenzung zum Hybrid Storage VPS

Nicht mit der DirectAdmin StorageBox verwechseln: HostBrr bietet auch Hybrid Storage VPS mit expliziten vCPU-/RAM-Werten, KVM und Root-Zugriff an. Deren Ressourcenstaffel darf nicht auf die normale StorageBox übertragen werden.

Quelle:

- https://hostbrr.com/hybrid-storage-vps.html

## Ergebnisstatus

Aktuell: `planned-verification`

Nach der späteren Inventarisierung werden einzelne Punkte auf `verified` hochgestuft. Die Performancewerte folgen erst in der später geplanten Testsuite.
