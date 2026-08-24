---
title: "10 Gbit/s: Was bedeutet das in der Praxis?"
category: performance
status: community-reported
last_reviewed: 2026-08-24
---

# 10 Gbit/s: Was bedeutet das in der Praxis?

HostBrr bewirbt die StorageBox-Plattform mit **10 Gbit/s Connectivity**. Das ist als Netzwerk-Anbindung der Plattform zu verstehen und **nicht** als Garantie, dass ein einzelner Benutzer oder ein einzelner Transfer 10 Gbit/s erreicht.

Offizielle Produktseite: [HostBrr StorageBoxes](https://hostbrr.com/storageboxes.html)

## Theoretisches Maximum

10 Gbit/s entsprechen brutto ungefähr 1,25 GB/s. In realen Transfers reduzieren Protokoll-Overhead, TCP, Verschlüsselung, Routing, Storage-I/O und Shared-Hosting-Ressourcen diesen Wert.

## Bisherige Community-Messwerte

In LowEndTalk wurden je nach Quelle und Standort sehr unterschiedliche Werte berichtet:

- rund **80–85 MB/s** bei Transfers aus Falkenstein bzw. aus gut angebundenen europäischen Rechenzentren;
- rund **100 MB/s** aus bestimmten EU-Standorten;
- rund **20–30 MB/s** aus einigen US-Rechenzentren;
- teilweise nur **1–3 MB/s** von privaten Anschlüssen mit ungünstigem Routing;
- Ende 2025 wurden bei rclone mit vier Standard-Transfers über **65 MB/s** für große Dateien berichtet.

Diese Werte sind Momentaufnahmen einzelner Nutzer und keine garantierten Leistungswerte.

## Warum 10 Gbit/s trotzdem sinnvoll sind

Eine schnelle Serveranbindung verhindert, dass schon der physische/virtuelle Uplink bei mehreren gleichzeitigen Benutzern zum offensichtlichen Engpass wird. Bei Shared Storage teilen sich jedoch viele Workloads Netzwerk, CPU und Storage-System.

## Wichtige Schlussfolgerung

Die Frage sollte nicht lauten:

> Schafft meine StorageBox 10 Gbit/s?

sondern:

> Welchen stabilen Durchsatz erreiche ich für meinen konkreten Workload zwischen meinem Standort und meiner StorageBox?

## Quellen

- [HostBrr StorageBoxes – offizielle Produktseite](https://hostbrr.com/storageboxes.html)
- [HostBrr StorageBox Bestellseite Deutschland](https://my.hostbrr.com/order/main/packages/storagebox/?group_id=63)
- [LowEndTalk – BF Storage Deals, Performanceberichte](https://lowendtalk.com/discussion/199617/hostbrr-bf-storage-deals-epyc-10-gbps-storage-vps-directadmin-storage-boxes-500gb-7-year/p27)
- [LowEndTalk – BF2025 StorageBox, weitere Messwerte](https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p13)
