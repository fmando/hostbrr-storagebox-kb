---
title: Performance
category: performance
status: maintained
last_reviewed: 2026-08-24
---
# Performance

HostBrr bewirbt die StorageBox-Plattform mit **10 Gbit/s Connectivity**, RAID-60-HDD-Speicher und NVMe-Cache. Diese Angaben beschreiben die Plattform, sind aber nicht mit garantiertem Durchsatz eines einzelnen Accounts gleichzusetzen.

Offizielle Produktseite: [HostBrr StorageBoxes](https://hostbrr.com/storageboxes.html)

## Kapitel

- [10 Gbit/s: Was bedeutet das in der Praxis?](10gbit-realitaet.md)
- [Große Dateien vs. viele kleine Dateien](grosse-kleine-dateien.md)
- [Latenz, Routing und Standort](latenz-routing.md)
- [Community-Messwerte](messwerte-community.md)

## Was die bisherigen Quellen zeigen

Community-Berichte reichen von nur wenigen MB/s bei ungünstigem Routing bis ungefähr 80–100 MB/s bei gut angebundenen Rechenzentrumsverbindungen. Gleichzeitig wird berichtet, dass hohe Latenz besonders Mounts und Workloads mit vielen kleinen Dateien stark beeinträchtigen kann.

Deshalb dokumentieren wir Performance nicht mit einer einzigen Zahl.

## Späteres Testprofil

Die eigentliche Testsuite wird bewusst später umgesetzt. Vorgesehen sind:

- Upload einer großen Datei
- Download einer großen Datei
- viele kleine Dateien
- rsync
- rclone
- parallele Transfers
- Latenz und Metadatenoperationen
- Cache-Verhalten
- Langzeittest statt nur kurzer Burst

Zu jedem Ergebnis dokumentieren wir Datum, Standort, Client-Anbindung, Tool-Version und relevante Parameter.

## Grundregel

Ein kurzer Schreibtest in einen lokalen/VFS/NVMe-Cache misst möglicherweise nur den Cache und nicht die StorageBox. Messungen müssen daher zwischen lokalem Cache, Netzwerktransfer und dauerhaftem Backend-I/O unterscheiden.
