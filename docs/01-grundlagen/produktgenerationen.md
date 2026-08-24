---
title: Produktgenerationen & historische Änderungen
category: grundlagen
status: maintained
last_reviewed: 2026-08-24
---
# Produktgenerationen & historische Änderungen

Bei HostBrr ändern sich Angebote, Hardwareplattformen und Standorte. Eine Aussage aus einem Forum ist deshalb nur dann wirklich nützlich, wenn klar ist, **wann und für welches Produkt** sie galt.

## Warum diese Trennung wichtig ist

In Community-Threads tauchen nebeneinander auf:

- DirectAdmin StorageBox
- StorageBox Reseller
- Hybrid Storage VPS
- Premium Storage VPS / StorageBrr
- HDD-Blockstorage-Add-ons

Diese Produkte können ähnliche Kapazitäten und Bandbreiten haben, technisch aber grundverschieden sein.

## Aktueller Referenzpunkt: 2026

Die aktuelle DirectAdmin-StorageBox wird von HostBrr als NVMe-gecachte HDD-Plattform auf RAID-60 mit 10-Gbit/s-Connectivity beschrieben. Die regulären Größen reichen derzeit von 500 GB bis 16 TB.

Für Deutschland liegen die regulären 1–16-TB-Angebote aktuell in Frankfurt; 500 GB wird im Bestellbereich in Falkenstein ausgewiesen.

## Historische Quellen

Historische Quellen werden nicht gelöscht. Stattdessen erhalten Aussagen nach Möglichkeit:

```yaml
observed: 2025-11
product: directadmin-storagebox
capacity: 2TB
location: unknown
status: historical-community-report
```

So kann später nachvollzogen werden, ob ein Verhalten durch eine Migration oder neue Produktgeneration verschwunden ist.

## Bekannte Änderungsarten

Besonders auf folgende Punkte achten:

1. **RAID-/Storage-Architektur** – ältere Angaben nicht ungeprüft auf die aktuelle RAID-60-Plattform übertragen.
2. **Standort** – ältere StorageBox-Angebote wurden teilweise an anderen Standorten beschrieben.
3. **Netzwerk** – frühere Angebote können andere Portgeschwindigkeiten nennen.
4. **LVE-/Account-Limits** – CPU, RAM, I/O und IOPS können server- oder paketabhängig sein.
5. **Software** – verfügbare Versionen von PHP, Borg, rsync usw. ändern sich.
6. **Preise und Promotions** – Black-Friday-Preise sind keine dauerhaften Listenpreise.

## Verwechslungsgefahr: Hybrid Storage VPS

Der Hybrid Storage VPS besitzt KVM, Root-Zugriff, ein NVMe-Systemlaufwerk und HDD-Blockstorage. Werte wie `4 vCPU / 8 GB RAM` bei einer 8-TB-Variante beziehen sich deshalb **nicht automatisch auf die 8-TB DirectAdmin StorageBox**.

Das ist eine zentrale Quellenregel dieser KB.

## Ziel

Langfristig soll diese Seite eine kleine Timeline enthalten:

| Zeitraum | Produktgeneration | Storage | Netzwerk | Standort | Hinweise |
|---|---|---|---|---|---|
| 2026 | aktuelle DirectAdmin StorageBox | RAID-60 + NVMe-Cache | 10 Gbit/s | Frankfurt/Falkenstein | offizielle aktuelle Referenz |

Weitere Zeilen werden erst ergänzt, wenn historische Angaben ausreichend sicher einem Produkt und Zeitraum zugeordnet werden können.

## Quellen

- [HostBrr StorageBox](https://hostbrr.com/storageboxes.html)
- [Aktueller StorageBox-Bestellbereich](https://my.hostbrr.com/order/main/packages/storagebox/?group_id=63)
- [HostBrr Hybrid Storage VPS](https://hostbrr.com/hybrid-storage-vps.html)
