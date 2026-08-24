---
title: Community-Messwerte
category: performance
status: community-reported
last_reviewed: 2026-08-24
---

# Community-Messwerte

Diese Seite sammelt Performancewerte aus öffentlichen Erfahrungsberichten. Sie sind **keine Benchmarks unter kontrollierten Bedingungen** und keine Leistungszusage von HostBrr.

| Zeitraum | Quelle → StorageBox | Methode/Workload | Berichteter Wert | Hinweis |
|---|---|---|---:|---|
| Dez. 2024 | EU/OVH → StorageBox | Transfer | ~100 MB/s | Nutzerbericht |
| Dez. 2024 | Hivelocity/Tempest NY → StorageBox | Transfer | ~20–30 MB/s | Routingabhängig |
| Dez. 2024 | Home nahe NYC → StorageBox | Transfer | ~1–2 MB/s | schlechtes Peering vermutet |
| Dez. 2024 | Hetzner FSN1 → HostBrr | rclone | ~80 MB/s | VPS als Transferhost |
| Dez. 2024 | HostBrr Falkenstein → StorageBox | rsync, 1-GB-Datei | ~85 MB/s | große Datei |
| Feb. 2025 | VPS im selben Land → StorageBox | Schreiben | 25,6 MB/s | Nutzerbericht |
| Feb. 2025 | VPS im selben Land ← StorageBox | Lesen | 30,5 MB/s | Nutzerbericht |
| Feb. 2025 | Home → StorageBox | Schreiben | 3,6 MB/s | Nutzerbericht |
| Feb. 2025 | Home ← StorageBox | Lesen | 1,3 MB/s | Nutzerbericht |
| Feb. 2025 | WebDAV | Upload | ~40 MB/s | anderer Nutzer meldete ~7 MB/s |
| Nov. 2025 | rsync.net → 2-TB StorageBox | rclone, 4 Transfers | >65 MB/s | große Dateien |
| Dez. 2025 | rclone/FTP-Mount | anfänglich | bis ~40 MB/s | später teils 1–2 MB/s gemeldet |

## Interpretation

Die Spannweite ist so groß, dass ein einzelner Wert wenig Aussagekraft besitzt. Besonders auffällig sind Unterschiede zwischen Rechenzentrums-zu-Rechenzentrums-Verbindungen und privaten Internetanschlüssen.

Für unsere spätere eigene Testsuite sollten wir daher nicht nur `MB/s` speichern, sondern mindestens:

- Datum/Uhrzeit
- StorageBox-Standort und Tarif
- Client-Standort und Provider
- RTT/Latenz
- Protokoll
- Tool + Version
- Parallelität
- Dateigröße bzw. Anzahl Dateien
- Upload oder Download
- Dauer
- Durchschnitt und möglichst Verlauf

## Quellen

- [LowEndTalk – Dezember 2024](https://lowendtalk.com/discussion/199617/hostbrr-bf-storage-deals-epyc-10-gbps-storage-vps-directadmin-storage-boxes-500gb-7-year/p27)
- [LowEndTalk – Februar 2025](https://lowendtalk.com/discussion/202755/hostbrr-valentine-offers-10-gbps-storagebox-7-year-cpanel-85-off-flash/p4)
- [LowEndTalk – November/Dezember 2025](https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p13)
- [LowEndTalk – Dezember 2025 Mount-Verhalten](https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p16)
