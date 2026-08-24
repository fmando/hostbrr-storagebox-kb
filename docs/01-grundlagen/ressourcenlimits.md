---
title: Ressourcenlimits und Fair Use
category: grundlagen
status: community-reported
last_reviewed: 2026-08-24
---
# Ressourcenlimits und Fair Use

Eine HostBrr StorageBox ist eine Shared-Hosting-Umgebung. Die nominelle Speicherkapazität und die 10-Gbit/s-Anbindung beschreiben daher nicht allein, wie viel CPU, RAM, I/O oder Prozesslaufzeit ein einzelner Account nutzen kann.

## Historisch berichtete LVE-Werte

Für eine 1-TB-StorageBox wurden im Februar 2025 in der HostBrr-Community folgende Werte genannt:

- 2 vCPU
- 2 GB RAM
- 100 MB/s I/O
- 1024 IOPS

Quelle: https://lowendtalk.com/discussion/203092/how-to-utilize-my-1-tb-hostbrr-storage-box-efficiently

**Wichtig:** Diese Werte sind Community-Angaben zu einer damaligen Produktgeneration und dürfen nicht ungeprüft auf aktuelle 2-TB- oder 8-TB-Boxen übertragen werden.

## Warum LVE wichtig ist

HostBrr verwendet bei seinem DirectAdmin-Hosting CloudLinux/LVE. LVE begrenzt Ressourcen pro Hosting-Account. Bei StorageBoxen müssen wir deshalb unter anderem unterscheiden zwischen:

- Netzwerkanbindung des Servers,
- I/O-Leistung des Storage-Arrays,
- LVE-I/O-/IOPS-Limit des Accounts,
- CPU- und RAM-Limit,
- Prozess-/Task-Limits,
- Limits für PHP und Datenbanken.

Offizielle HostBrr-Seite zu DirectAdmin/LVE: https://hostbrr.com/directadmin.html

## Noch zu verifizieren

Auf aktuellen Boxen sollten später erfasst werden:

1. CPU-Limit
2. RAM-Limit
3. I/O-Limit
4. IOPS-Limit
5. Entry Processes / Prozesse / Tasks
6. maximale Cronjob-Laufzeit
7. PHP memory_limit und max_execution_time
8. Upload-/POST-Limits
9. Datenbanklimits
10. Anzahl paralleler SSH/SFTP-Verbindungen
11. ausgehende und eingehende Netzwerkbeschränkungen
12. Verhalten lang laufender Prozesse nach Ende einer SSH-Sitzung

## Lang laufende Dienste

Community-Berichte aus Ende 2025 beschreiben eine eingeschränkte Ausführungsumgebung: selbst gestartete Prozesse können beim Ende der SSH-Verbindung beendet werden und frei erreichbare eigene Dienste/Ports sind nicht vorgesehen. Das spricht gegen Anwendungen wie einen direkt gestarteten MinIO-Server, Docker-Daemons oder eigene Netzwerkdienste.

Quelle: https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p18

Daraus folgt vorläufig:

> Die StorageBox sollte als Storage-/Shared-Hosting-Plattform betrachtet werden, nicht als kleiner VPS.

## 2-TB-vs.-8-TB-Vergleich

Später sollten identische Werte auf einer aktuellen 2-TB- und 8-TB-Box erfasst werden. Damit lässt sich klären, ob die Tarife außer Kapazität und Transfer auch unterschiedliche LVE-Ressourcen erhalten.
