---
title: Große Dateien vs. viele kleine Dateien
category: performance
status: community-reported
last_reviewed: 2026-08-24
---

# Große Dateien vs. viele kleine Dateien

Eine StorageBox kann bei großen sequenziellen Transfers schnell sein und sich gleichzeitig bei hunderttausenden kleinen Dateien deutlich langsamer anfühlen.

## Große Dateien

Backuparchive, Videos und große Disk-Images sind für HDD-basierte Storage-Systeme vergleichsweise günstig: wenige Metadatenoperationen und überwiegend sequenzieller I/O.

Community-Berichte zeigen bei gutem Routing mehrfach Werte im Bereich von etwa 65–100 MB/s für große Transfers.

## Kleine Dateien

Bei vielen kleinen Dateien entstehen dagegen zahlreiche Operationen:

- Verzeichnis auflisten
- Dateiattribute lesen
- Datei öffnen
- übertragen
- schließen
- Zeitstempel/Metadaten aktualisieren

Bei SFTP oder anderen Remote-Dateisystemen kommt die Netzwerklatenz hinzu. Dadurch kann ein Dataset mit 100.000 kleinen Dateien wesentlich länger dauern als eine einzelne Datei mit derselben Gesamtgröße.

## NVMe-Cache

HostBrr beschreibt die Plattform als HDD RAID-60 mit Gen4-NVMe-Cache. Community-Berichte nach Hardware-Migrationen Ende 2025 beschreiben insbesondere kleinere Schreibvorgänge und inkrementelle Backups als deutlich schneller.

Der Cache verwandelt das System aber nicht in ein reines NVMe-Dateisystem. Cache-Hits und kurze Bursts dürfen deshalb nicht mit dauerhaftem HDD-Durchsatz verwechselt werden.

## Mounts sind besonders empfindlich

Bei `rclone mount` über SFTP müssen Metadaten häufig über die Netzwerkverbindung abgefragt werden. Ein Nutzer berichtete deshalb bei einem Immich-Dataset über langsame inkrementelle Vorgänge und wechselte zu JuiceFS mit lokalem Cache.

Offizielle Dokumentation:

- [rclone mount](https://rclone.org/commands/rclone_mount/)
- [rclone VFS File Caching](https://rclone.org/commands/rclone_mount/#vfs-file-caching)
- [JuiceFS Cache](https://juicefs.com/docs/community/cache_management/)

## Konsequenz für Backups

Wo möglich, viele kleine Dateien vor dem Offsite-Transfer zu einem Backupformat zusammenzufassen oder ein deduplizierendes Backupwerkzeug zu verwenden, kann wesentlich effizienter sein als ein naiver Dateifür-Datei-Mirror.

## Quellen

- [HostBrr StorageBoxes](https://hostbrr.com/storageboxes.html)
- [LowEndTalk – StorageBox mit JuiceFS/rclone und NVMe-Cache](https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p15)
