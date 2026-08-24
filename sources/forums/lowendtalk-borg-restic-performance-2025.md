---
title: "HostBrr StorageBox – Borg/Restic-Verfügbarkeit und Performanceberichte"
source_type: forum
source_name: LowEndTalk
retrieved: 2026-08-24
reliability: community
storagebox_generation: mixed-2024-2025
location: unknown
---
# Borg und Restic auf der StorageBox

## Fundstellen

- https://lowendtalk.com/discussion/199617/hostbrr-bf-storage-deals-epyc-10-gbps-storage-vps-directadmin-storage-boxes-500gb-7-year/p10
- https://lowendtalk.com/discussion/192011/hostbrr-directadmin-cpanel-hosting-reseller-100-nvme-litespeed-eu-usa-sg-upto-50-off/p25

## Relevante Aussagen

Im November 2024 wurde auf die Frage nach Borg-Unterstützung für die DirectAdmin StorageBox geantwortet, Borg 1.2.4 sei installiert, allerdings damals nicht die aktuelle stabile Version.

Ein weiterer Nutzerbericht von November 2025 beschreibt Restic auf einer neuen StorageBox als langsam und Borg als deutlich schneller. Das ist ein einzelner Erfahrungswert und kein reproduzierbarer Benchmark.

## Einordnung

- Borg-Verfügbarkeit ist historisch gut belegt, aber für die aktuelle 2026er Plattform noch zu prüfen.
- Die installierte Borg-Version darf nicht aus dem 2024er Bericht auf heutige Accounts übertragen werden.
- Restic sollte nicht als ungeeignet markiert werden; die Performance muss mit einem kontrollierten Test untersucht werden.

## Testbedarf

```bash
which borg
borg --version
which restic
restic version
```

Danach identische Testdaten mit Borg und Restic sichern und Restore-Geschwindigkeit vergleichen.
