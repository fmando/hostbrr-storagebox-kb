---
title: "Was ist die HostBrr StorageBox?"
category: grundlagen
status: official-plus-community
last_reviewed: 2026-08-24
storagebox_generation: current-2026
---

# Was ist die HostBrr StorageBox?

Die HostBrr StorageBox ist **kein VPS und kein Root-Server**, sondern eine Shared-Hosting-Umgebung, die auf große und günstige Speicherkapazitäten ausgerichtet ist. HostBrr beschreibt die aktuelle Plattform als HDD-Speicher in RAID-60 mit NVMe-Cache. Anders als klassische reine Storage-Ziele enthält das Produkt zusätzlich einen Webhosting-Stack mit DirectAdmin, LiteSpeed, PHP und Softaculous.

## Wofür ist sie gedacht?

Die naheliegenden Einsatzbereiche sind:

- Offsite-Backups
- große Dateiarchive
- Medienarchive
- Remote Storage für Server und Clients
- persönliche Cloud-Anwendungen wie Nextcloud
- Websites mit hohem Speicherbedarf und moderaten I/O-Anforderungen

Für datenbank- oder IOPS-intensive Anwendungen ist die Architektur weniger geeignet als reines NVMe-Hosting.

## Zugriffsmöglichkeiten

HostBrr dokumentiert aktuell:

- SSH
- FTP
- FTPS
- rsync
- DirectAdmin File Manager

SFTP ergibt sich über den SSH-Zugang und wird auch in Community-Berichten praktisch verwendet. rclone funktioniert nach mehreren Nutzerberichten über das SFTP-Backend.

## Aktuelle deutsche Größen

| Speicher | inkludierter Transfer | aktuell gelisteter Standort |
|---:|---:|---|
| 500 GB | 2.5 TB | Falkenstein |
| 1 TB | 5 TB | Frankfurt |
| 2 TB | 10 TB | Frankfurt |
| 4 TB | 20 TB | Frankfurt |
| 8 TB | 40 TB | Frankfurt |
| 16 TB | 80 TB | Frankfurt |

Die größeren deutschen Pakete werden derzeit mit 10-Gbit/s-Konnektivität angeboten. Das ist **keine Garantie für 10 Gbit/s realen Dateitransfer**. Durchsatz hängt unter anderem von Storage-Last, Protokoll, Dateigrößen, Quellsystem, Routing und Peering ab.

## Was bekommt man nicht?

Insbesondere gibt es bei der StorageBox:

- keinen Root-Zugang
- keine frei administrierbare VM
- keine dedizierten CPU-/RAM-Ressourcen wie bei einem VPS
- keinen allgemeinen Anspruch darauf, beliebige Serverprogramme installieren zu können

HostBrr grenzt die StorageBox selbst von seinem Hybrid Storage VPS ab: Der Hybrid-VPS ist eine KVM-VM mit Root-Zugang und dedizierten Ressourcen; die StorageBox läuft auf geteilter Infrastruktur.

## DirectAdmin und Anwendungen

DirectAdmin dient nicht nur als Dateimanager. Die aktuelle Produktbeschreibung nennt LiteSpeed, PHP und Softaculous. HostBrr bewirbt insbesondere die Installation von Nextcloud bzw. ownCloud.

Community-Erfahrungen zeigen jedoch einen wichtigen Architekturpunkt: Nur weil eine Anwendung installiert werden kann, ist die StorageBox nicht automatisch die beste Plattform dafür. Für I/O-intensive Datenbanken oder anspruchsvolle Nextcloud-Installationen kann ein separater VPS sinnvoller sein, während die StorageBox ausschließlich die großen Datenmengen hält.

## Sicherheit

RAID-60 erhöht die Verfügbarkeit gegenüber einzelnen Laufwerksausfällen, ist aber **kein Backup**. Sensible Backup-Daten sollten nach Möglichkeit bereits auf dem Quellsystem verschlüsselt werden. Dafür kommen beispielsweise Restic, Borg oder rclone crypt in Betracht; die konkrete Kompatibilität der aktuellen StorageBox-Generation wird in eigenen Tests dokumentiert.

## Historische Angaben nicht mit aktuellen vermischen

Ältere HostBrr-StorageBoxes wurden in Community-Beiträgen teilweise als RAID-6 beschrieben und hatten andere LVE-/I/O-Limits. Deshalb kennzeichnet diese Knowledge Base Quellen immer mit Datum bzw. Produktgeneration. Werte aus 2024/2025 werden nicht automatisch als Eigenschaften der aktuellen 2026er Plattform übernommen.

## Quellen

- HostBrr StorageBox Produktseite: https://hostbrr.com/storageboxes.html
- HostBrr Deutschland-Bestellseite: https://my.hostbrr.com/order/main/packages/storagebox/?group_id=63
- HostBrr Hybrid Storage VPS / Produktabgrenzung: https://hostbrr.com/hybrid-storage-vps.html
- LowEndTalk: How to Utilize My 1 TB HostBrr Storage Box Efficiently?
- LowEndTalk: HostBrr Storage Boxes – Any experiences with them?
- LowEndTalk: HostBrr Storage Box – How safe is it?

Siehe zusätzlich die aufbereiteten Einträge unter `sources/`.
