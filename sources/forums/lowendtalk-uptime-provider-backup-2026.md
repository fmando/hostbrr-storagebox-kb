---
title: "StorageBox uptime, RAID-60 and provider backup statements"
source_type: forum
source_name: LowEndTalk
url: "https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p22"
published: 2026-01
retrieved: 2026-08-24
reliability: community-provider-mixed
storagebox_generation: current-at-2026-01
location: Germany
---

# Relevante Aussagen

## Verfügbarkeit

Ein Nutzer mit permanenten SSHFS-Mounts auf 7–10 Maschinen erinnert sich für das Vorjahr an drei relevante Downtimes, die längste ungefähr 1–2 Stunden. Ansonsten bewertet er die Verfügbarkeit sehr positiv.

Dies ist eine einzelne Nutzererfahrung und keine unabhängige SLA-Messung.

## Keine zusätzliche Sicherung durch HostBrr

Auf die direkte Frage, ob HostBrr die StorageBoxen sichert, antwortete Providervertreter `labze` mit Nein und wies darauf hin, dass dies auch nicht behauptet werde.

Diese Aussage ist für das Sicherheitsmodell der KB besonders wichtig.

## RAID-Aufbau

Im selben Gespräch beschreibt `labze` den Storage-Aufbau als RAID-60 mit drei zugrunde liegenden RAID-6-Arrays. Je nach Verteilung könnten mehrere Laufwerksausfälle toleriert werden.

## Konsequenz

RAID-Redundanz darf nicht mit Backup verwechselt werden. Kritische Daten benötigen mindestens eine weitere unabhängige Kopie.
