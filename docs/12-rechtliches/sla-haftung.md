---
title: SLA, Verfügbarkeit & Haftung
category: rechtliches
status: maintained
last_reviewed: 2026-08-24
---
# SLA, Verfügbarkeit & Haftung

## Gibt es ein StorageBox-Uptime-SLA?

In den aktuell veröffentlichten Terms of Service konnten wir **keine konkrete Uptime-Garantie oder StorageBox-spezifische SLA-Regelung** finden.

Das ist wichtig, weil HostBrr auf einzelnen VPS-Produktseiten ausdrücklich eine **99,9 % Uptime SLA** nennt. Diese Angabe darf nicht automatisch auf die StorageBox übertragen werden.

Solange HostBrr keine StorageBox-spezifische SLA veröffentlicht oder im Bestellprozess zusichert, behandeln wir die StorageBox als Dienst **ohne dokumentierte vertragliche Uptime-Zahl**.

## Produktversprechen vs. SLA

Die StorageBox-Seite wirbt mit Zuverlässigkeit, RAID-60, NVMe-Cache und 10-Gbit/s-Anbindung. Das sind Produkteigenschaften bzw. Marketingaussagen, aber keine automatisch gleichbedeutende Entschädigungs-SLA.

## Backups

HostBrr weist auf einer VPS-Seite ausdrücklich darauf hin, dass StorageBoxen **keine automatischen Backups** erhalten. Das deckt sich mit früheren Provider-Aussagen in der Community.

Konsequenz:

```text
RAID-60 = Schutz gegen bestimmte Laufwerksausfälle
StorageBox = Offsite-Speicherziel
Provider-Backup = nicht enthalten
zweite eigene Kopie = weiterhin nötig
```

## Datenverlust und Haftung

Die Terms begrenzen die Haftung grundsätzlich auf die für den betroffenen Dienst in den vorhergehenden zwölf Monaten gezahlten Gebühren. Indirekte Schäden, Betriebsunterbrechung und Datenverlust werden grundsätzlich ausgeschlossen, soweit dies rechtlich zulässig ist.

Zwingende Verbraucherrechte sowie Haftung für Vorsatz, grobe Fahrlässigkeit und sonstige gesetzlich nicht ausschließbare Fälle bleiben laut Terms bestehen.

## Force Majeure

Bei Ereignissen außerhalb der zumutbaren Kontrolle — etwa Naturkatastrophen, Strom-/Carrier-Ausfällen oder DDoS — sieht HostBrr eine Haftungsbefreiung für Verzögerungen vor. Dauert ein solches Ereignis länger als 30 aufeinanderfolgende Tage und verhindert die Leistungserbringung, können beide Seiten den betroffenen Dienst beenden; die Terms sehen dann eine anteilige Erstattung des nicht gelieferten vorausbezahlten Zeitraums vor.

## Konsequenz für die eigene Architektur

Wer eine hohe Verfügbarkeit benötigt, sollte nicht versuchen, diese allein aus einer einzelnen StorageBox abzuleiten. Sinnvoller ist beispielsweise:

- primäre Daten auf einem eigenen Server/NAS
- StorageBox als Offsite-Kopie
- bei kritischen Daten eine weitere unabhängige Kopie
- dokumentierter Restore-Prozess
- Monitoring der StorageBox-Erreichbarkeit

## Offene Frage für v1.0

Wir sollten HostBrr noch gezielt per Support fragen, ob für aktuelle DirectAdmin-StorageBoxen eine unveröffentlichte bzw. beim Bestellprozess geltende Uptime-SLA oder Service-Credit-Regel existiert.
