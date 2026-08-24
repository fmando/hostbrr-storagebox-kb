---
title: Latenz, Routing und Standort
category: performance
status: community-reported
last_reviewed: 2026-08-24
---

# Latenz, Routing und Standort

Die Entfernung allein erklärt die Performance einer StorageBox nicht. Entscheidend sind zusätzlich Routing, Peering, Paketverlust, TCP-Verhalten und das verwendete Protokoll.

## Auffällige Community-Erfahrungen

Mehrere Nutzer berichten, dass dieselbe StorageBox aus einem europäischen VPS deutlich schneller erreichbar war als vom heimischen Internetanschluss. Ein Bericht nennt ungefähr 80 MB/s über einen Hetzner-VPS in Falkenstein, während andere Pfade wesentlich langsamer waren.

Ende 2025 wurde außerdem ein rclone-Mount mit ungefähr 150 ms Latenz als problematisch für einen Immich-Workload beschrieben. Ein anderer Nutzer betonte, dass viele kleine Dateien bei rclone mount stark von der Latenz abhängen.

## Warum Latenz kleine Dateien stärker trifft

Bei sequenziellen großen Transfers kann TCP die Verbindung gut auslasten. Viele voneinander abhängige Metadatenoperationen verursachen dagegen zahlreiche Roundtrips. Bei 100 ms RTT können schon wenige zusätzliche Roundtrips pro Datei massiv ins Gewicht fallen.

## Diagnose

Vom Client aus:

```bash
ping STORAGEBOX_HOST
traceroute STORAGEBOX_HOST
```

Falls ICMP blockiert ist, können `mtr` oder TCP-basierte Messungen hilfreicher sein.

Projektseiten:

- [MTR](https://github.com/traviscross/mtr)
- [iperf3](https://github.com/esnet/iperf)

`iperf3` ist nur verwendbar, wenn auf beiden Enden ein passender Prozess laufen kann. Auf einer Shared-Hosting-StorageBox ist das nicht vorauszusetzen.

## Relay-VPS als Sonderfall

Community-Nutzer berichten, dass ein gut angebundener VPS als Zwischenstation bei schlechtem ISP-Routing den effektiven Durchsatz verbessern kann. Das ist kein universeller Fix und verursacht zusätzliche Komplexität sowie eventuell Traffic-Kosten.

Schema:

```text
Home/Server
    ↓
schlechter direkter Pfad
    ↓
StorageBox

Alternative:

Home/Server → gut angebundener VPS → StorageBox
```

## Standort der deutschen StorageBoxen

Die aktuelle Bestellseite nennt für die 500-GB-Variante Falkenstein und für 1 TB bis 16 TB Frankfurt. Das kann für Routing- und Latenzvergleiche relevant sein.

## Quellen

- [HostBrr StorageBox Deutschland](https://my.hostbrr.com/order/main/packages/storagebox/?group_id=63)
- [LowEndTalk – Falkenstein/Peering-Messwerte](https://lowendtalk.com/discussion/199617/hostbrr-bf-storage-deals-epyc-10-gbps-storage-vps-directadmin-storage-boxes-500gb-7-year/p27)
- [LowEndTalk – Latenz und rclone mount](https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p13)
- [LowEndTalk – Relay-VPS bei schlechtem Routing](https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p19)
