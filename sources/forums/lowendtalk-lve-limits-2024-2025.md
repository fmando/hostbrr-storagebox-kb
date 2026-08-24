---
title: "Historical StorageBox LVE limits"
source_type: forum
source_name: LowEndTalk
url: "https://lowendtalk.com/discussion/199617/hostbrr-bf-storage-deals-epyc-10-gbps-storage-vps-directadmin-storage-boxes-500gb-7-year/p5"
published: 2024-11
retrieved: 2026-08-24
reliability: provider-community
storagebox_generation: 2024-2025
location: Germany
---

# Historische Angaben

Providervertreter `labze` nannte für damalige DirectAdmin StorageBox-Angebote folgende LVE-Limits:

- 2 vCore
- 2 GB RAM
- 100 MB/s I/O
- 1024 IOPS

Ein späterer Angebotsthread vom Februar/März 2025 wiederholt dieselben Werte. Dort erklärt `labze` außerdem, zwischen 500-GB- und 1-TB-Paketen gebe es keinen Geschwindigkeitsunterschied.

Weitere Quelle:

https://lowendtalk.com/discussion/202755/hostbrr-valentine-offers-10-gbps-storagebox-7-year-cpanel-85-off-flash/p5

## Wichtige Einschränkung

Diese Werte sind **historisch**. Die aktuelle offizielle StorageBox-Produktseite nennt 2026 keine konkreten CPU-, RAM-, I/O- oder IOPS-Limits.

Sie dürfen deshalb nicht ungeprüft als aktuelle Limits für 2-TB- oder 8-TB-Boxen verwendet werden.

## Zu verifizieren

Auf beiden Testboxen später prüfen:

- DirectAdmin Resource Usage / LVE
- CPU-Limit
- RAM-Limit
- I/O-Limit
- IOPS-Limit
- Entry Processes / Tasks
- Prozesslimit
