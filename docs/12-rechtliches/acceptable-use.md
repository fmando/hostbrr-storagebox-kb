---
title: Acceptable Use & Fair Use
category: rechtliches
status: official
last_reviewed: 2026-08-24
---
# Acceptable Use & Fair Use

Quelle: https://hostbrr.com/aup.html — Stand laut HostBrr: Mai 2026.

## Grundsatz

Die AUP gilt laut HostBrr für alle Dienste und soll Missbrauch sowie Beeinträchtigungen anderer Kunden verhindern.

## Ressourcenmissbrauch

HostBrr definiert Resource Abuse als Nutzung von CPU, I/O, Netzwerk oder Datenbankressourcen in einem Umfang, der andere Kunden auf demselben physischen Node oder Cluster beeinträchtigt.

Untersagt sind insbesondere dauerhaft schwere, ressourcenintensive Aktivitäten außerhalb der Fair-Share-Zuteilung.

## Fair Usage

Gemeinsam genutzte Ressourcen wie CPU-Zeit, Netzwerkinterfaces und Disk-I/O werden dynamisch zwischen Kunden geteilt. Bei anhaltender Sättigung mit spürbaren Auswirkungen auf andere Kunden will HostBrr zunächst eine Reduzierung verlangen; in kritischen Fällen können Dienste vorübergehend gestoppt oder suspendiert werden.

Für die StorageBox ist das relevant bei:

- sehr hoher Transferparallelität
- vielen gleichzeitigen SFTP-/rsync-Prozessen
- dauerhaft hoher I/O-Last
- massiver Zahl kleiner Dateien und Metadatenoperationen
- CPU-intensiver Verschlüsselung/Kompression direkt auf der Box

## Verbotene Nutzungen

Die AUP untersagt unter anderem:

- nicht autorisierte urheberrechtlich geschützte Inhalte
- öffentliche offene Proxies/VPNs
- DDoS-/Stress-Tools
- Spam und Bulk-Mail
- Portscans und unautorisierte Penetrationstests
- Brute-Force-Angriffe
- Malware, Ransomware, Phishing und Botnet/C2
- nicht autorisiertes Cryptomining
- bestimmte Anonymisierungsdienste und offene DNS-Resolver

Die vollständige Liste steht in der offiziellen AUP.

## Wichtige Abgrenzung

Die Terms verbieten Backup-Repositories und Remote-Storage ausdrücklich auf **normalem Shared- und Reseller-Webhosting**. Gleichzeitig führt die AUP Media-/Storage-Workloads ausdrücklich als etwas auf, das auf einen VPS oder eine **StorageBox** gehört.

Damit darf diese Shared-Webhosting-Einschränkung nicht versehentlich auf das eigens dafür angebotene StorageBox-Produkt übertragen werden.

## Was wir noch klären wollen

Für eine aktuelle StorageBox fehlen weiterhin konkrete veröffentlichte Schwellenwerte dafür, ab wann Fair Use greift. Historische LVE-Werte behandeln wir daher separat und nicht als aktuellen Vertragswert.
