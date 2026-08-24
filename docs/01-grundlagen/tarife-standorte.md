---
title: Tarife, Standorte & Produktreferenz
category: grundlagen
status: maintained
last_reviewed: 2026-08-24
---
# Tarife, Standorte & Produktreferenz

Diese Seite hält die **aktuell öffentlich angebotenen HostBrr StorageBox-Tarife** fest. Preise und Produktdaten können sich ändern; deshalb gehört zu jeder Preisangabe ein Prüfdatum.

## Aktuelle Deutschland-Tarife (Stand 24.08.2026)

| Kapazität | Inklusivtransfer | Netzwerk | Standort |
|---:|---:|---:|---|
| 500 GB | 2,5 TB | 10-Gbit/s-Plattform* | Falkenstein |
| 1 TB | 5 TB | 10 Gbit/s | Frankfurt |
| 2 TB | 10 TB | 10 Gbit/s | Frankfurt |
| 4 TB | 20 TB | 10 Gbit/s | Frankfurt |
| 8 TB | 40 TB | 10 Gbit/s | Frankfurt |
| 16 TB | 80 TB | 10 Gbit/s | Frankfurt |

\* Die allgemeine Produktseite nennt 10-Gbit/s-Connectivity für alle Größen; im Bestelltext der 500-GB-Variante wird die Geschwindigkeit derzeit nicht noch einmal explizit hinter dem Transferkontingent angegeben.

Die Transfermenge skaliert damit aktuell nach der einfachen Regel **5 × Speicherkapazität**.

## Aktuelle Monatspreise

HostBrr nennt auf seiner Produkt-/Vergleichsseite vom Juli 2026 folgende reguläre Größenordnung:

| Tarif | Monatspreis | Preis pro TB/Monat |
|---:|---:|---:|
| 1 TB | 2,50 € | 2,50 € |
| 2 TB | 4,50 € | 2,25 € |
| 4 TB | 8,50 € | 2,13 € |
| 8 TB | 16,50 € | 2,06 € |
| 16 TB | 32,00 € | 2,00 € |

Die 500-GB-Variante wird separat bzw. teilweise mit Jahrespreis beworben. Aktionscodes, Black-Friday-Angebote und jährliche Vorauszahlung können die effektiven Preise deutlich verändern. Deshalb verwenden wir Aktionspreise **nicht** als dauerhafte Tarifreferenz.

## Gemeinsame Ausstattung

Die aktuelle Produktseite beschreibt die StorageBox-Plattform als:

- NVMe-gecachte HDD-Arrays
- RAID-60
- DirectAdmin
- SSH / FTP / rsync
- kostenlose SSL-Zertifikate
- Softaculous
- 10-Gbit/s-Connectivity
- unbegrenzte Domains/Subdomains laut Tarifbeschreibung

Wichtig: **10 Gbit/s ist die Connectivity der Plattform und keine garantierte Einzeltransfer-Geschwindigkeit.** Siehe Performance-Kapitel.

## Deutschland: Frankfurt und Falkenstein

HostBrr nennt generell beide deutschen Standorte. Beim aktuellen StorageBox-Angebot ist die Aufteilung bemerkenswert:

- 500 GB → Falkenstein
- 1–16 TB → Frankfurt

Damit dürfen Messwerte unterschiedlicher Tarife nicht automatisch miteinander verglichen werden, ohne auch den Standort zu dokumentieren.

## StorageBox ≠ Hybrid Storage VPS

HostBrr bietet parallel einen **Hybrid Storage VPS** an. Dieser darf nicht mit der DirectAdmin StorageBox vermischt werden.

| Merkmal | StorageBox | Hybrid Storage VPS |
|---|---|---|
| Umgebung | Shared Hosting | KVM VPS |
| Verwaltung | DirectAdmin | eigenes Betriebssystem |
| Root | nein | ja |
| Storage | NVMe-gecachte HDD-Plattform | NVMe-Boot + HDD-Blockstorage |
| Anwendungen | Shared-Hosting-Stack | frei installierbar |
| IPv4/IPv6 | kein eigener VPS-Stack | eigener IPv4 + IPv6/64 laut Tarif |

Historische Community-Threads enthalten häufig Angaben zu beiden Produktfamilien. Beim Quellenimport muss deshalb immer geprüft werden, **welches Produkt tatsächlich gemeint ist**.

## Reseller-StorageBox

Zusätzlich existiert eine StorageBox-Reseller-Produktgruppe mit mehreren DirectAdmin-Accounts. Diese gehört nicht zur normalen Single-Account-StorageBox und wird in der KB separat behandelt, sobald genügend belastbare Informationen vorliegen.

## Produktgenerationen

Wir unterscheiden vorläufig:

### Ältere Generationen / historische Angebote

Ältere Forenbeiträge können andere Standorte, RAID-Angaben, Ressourcenlimits, Preise und Serverkonfigurationen nennen. Solche Daten bleiben wertvoll, werden aber mit Datum und Produktgeneration gekennzeichnet.

### Aktuelle Generation (2026)

Die öffentliche Produktseite nennt aktuell eine gemeinsame **RAID-60-Plattform mit NVMe-Cache** und 10-Gbit/s-Connectivity. HostBrr schreibt, dass alle Größen auf derselben Plattform laufen und die Bandbreite mit der Kapazität skaliert.

Das bedeutet noch nicht, dass jede Box zwingend auf demselben physischen Node liegt oder identische Account-Limits besitzt.

## Späterer 2-TB-vs.-8-TB-Vergleich

Für einen belastbaren Tarifvergleich sollten auf beiden Boxen zusätzlich festgehalten werden:

- Bestelldatum / Tarifbezeichnung
- Hostname
- Standort
- DirectAdmin-Version
- sichtbare Account-Limits
- Shell-Limits
- verfügbare Programme
- Quota
- Netzwerk-/Transferkontingent
- Node-/Plattformhinweise, soweit ohne invasive Methoden sichtbar
- identische Performance-Messungen

Damit lässt sich klären, ob 2 TB und 8 TB nur Kapazität und Transfer unterscheiden oder ob weitere technische Unterschiede existieren.

## Quellen

- [HostBrr StorageBox – offizielle Produktseite](https://hostbrr.com/storageboxes.html)
- [HostBrr StorageBox – aktueller Bestellbereich](https://my.hostbrr.com/order/main/packages/storagebox/?group_id=63)
- [HostBrr: Storagebox vs. Hetzner Storage Box vs. Amazon S3 (Juli 2026)](https://hostbrr.com/storagebox-vs-hetzner-vs-s3.html)
- [HostBrr – Standorte und Anbieterinformationen](https://hostbrr.com/about.html)
- [HostBrr Hybrid Storage VPS](https://hostbrr.com/hybrid-storage-vps.html)
