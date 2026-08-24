---
title: Produkt- und Tarifreferenz
category: grundlagen
status: maintained
last_reviewed: 2026-08-24
---
# Produkt- und Tarifreferenz

Diese Seite dokumentiert die reguläre HostBrr **DirectAdmin StorageBox**. Andere HostBrr-Produkte werden getrennt behandelt.

## Deutschland

Aktuell öffentlich angebotene Größen:

| Speicher | Transfer | Connectivity | Standort |
|---:|---:|---:|---|
| 500 GB | 2,5 TB | laut Produktfamilie 10 Gbit/s | Falkenstein |
| 1 TB | 5 TB | 10 Gbit/s | Frankfurt |
| 2 TB | 10 TB | 10 Gbit/s | Frankfurt |
| 4 TB | 20 TB | 10 Gbit/s | Frankfurt |
| 8 TB | 40 TB | 10 Gbit/s | Frankfurt |
| 16 TB | 80 TB | 10 Gbit/s | Frankfurt |

Offizielle Bestellseite:

- https://my.hostbrr.com/order/main/packages/storagebox/?group_id=63

## Gemeinsame Merkmale

HostBrr nennt für die Produktfamilie unter anderem:

- DirectAdmin
- SSH
- FTP/FTPS
- rsync
- kostenlose SSL-Zertifikate
- Softaculous
- RAID-60-HDD-Storage
- NVMe-Cache
- 10-Gbit/s-Connectivity

Produktseite:

- https://hostbrr.com/storageboxes.html

## Bandbreitenskalierung

HostBrr beschreibt die regulären Tarife mit einem Transferkontingent von ungefähr dem Fünffachen der gebuchten Speicherkapazität.

Beispiele:

- 2 TB Speicher → 10 TB Transfer
- 8 TB Speicher → 40 TB Transfer

Das Transferkontingent ist nicht mit der Portgeschwindigkeit zu verwechseln.

## Preise

Preise können sich ändern und Aktionen können deutlich von regulären Preisen abweichen. Deshalb sollte bei Preisangaben immer das Abrufdatum dokumentiert werden.

Eine HostBrr-Vergleichsseite nennt derzeit als reguläre Monatswerte:

| Speicher | Preis/Monat |
|---:|---:|
| 1 TB | €2,50 |
| 2 TB | €4,50 |
| 4 TB | €8,50 |
| 8 TB | €16,50 |
| 16 TB | €32,00 |

Die 500-GB-Variante wird dort als Jahresangebot geführt.

Quelle:

- https://hostbrr.com/storagebox-vs-hetzner-vs-s3.html

## Was die öffentliche Tarifliste nicht beantwortet

Nicht vollständig öffentlich ausgewiesen sind insbesondere:

- CPU-/LVE-Limit der StorageBox-Accounts
- RAM-Limit
- I/O-Limit
- IOPS-Limit
- Prozesslimits
- Entry Processes
- Zahl paralleler SSH/SFTP-Verbindungen
- genaue NVMe-Cache-Architektur
- Anzahl Accounts pro Storage-Node

Diese Punkte dürfen nicht aus HostBrrs normalem DirectAdmin-Webhosting oder Hybrid Storage VPS abgeleitet werden.

## Abgrenzung: DirectAdmin Webhosting

Das normale HostBrr DirectAdmin-Webhosting ist ein anderes Produkt mit NVMe-Gen4-Speicher und explizit ausgewiesenen LVE-Limits. Diese Werte sind **keine StorageBox-Spezifikationen**.

- https://hostbrr.com/directadmin.html

## Abgrenzung: Hybrid Storage VPS

Hybrid Storage VPS verwendet KVM, bietet Root-Zugriff und besitzt explizit zugewiesene vCPU-, RAM-, NVMe- und HDD-Ressourcen. Auch diese Werte dürfen nicht auf die DirectAdmin StorageBox übertragen werden.

- https://hostbrr.com/hybrid-storage-vps.html

## Abgrenzung: StorageBox Reseller

HostBrr bietet außerdem StorageBox-Reseller-Pakete mit mehreren DirectAdmin-Accounts an. Auch diese bilden eine eigene Produktklasse.

- https://my.hostbrr.com/order/main/packages/storagebox/?group_id=64

## Quellenregel

Bei jedem gefundenen HostBrr-Angebot zuerst prüfen:

1. DirectAdmin StorageBox?
2. StorageBox Reseller?
3. Hybrid Storage VPS?
4. anderes Webhosting-/Storageprodukt?
5. aktuelles Angebot oder historischer Promotion-Thread?

Erst danach technische Werte in die Wissensdatenbank übernehmen.
