---
title: 3-2-1-Strategie mit HostBrr
category: sicherheit
status: maintained
last_reviewed: 2026-08-24
---
# 3-2-1-Strategie mit HostBrr

Die klassische 3-2-1-Regel bedeutet:

- **3 Kopien** der Daten insgesamt,
- auf **2 unterschiedlichen Medien bzw. Speicherorten**,
- davon **1 Kopie offsite**.

CISA verweist ausdrücklich auf diese Strategie als sinnvolle Grundlage für die Wiederherstellung nach Ransomware-Vorfällen.

## Beispiel mit HostBrr

```text
Produktivdaten
    │
    ├── lokale Backup-Kopie
    │      z. B. NAS / Proxmox-Storage
    │
    └── verschlüsselte Offsite-Kopie
           HostBrr StorageBox
```

Für besonders wichtige Daten sollte zusätzlich eine Kopie existieren, die **nicht dauerhaft schreibbar** vom Produktionssystem erreichbar ist.

## Gute Rollenverteilung

### Lokal

- schneller Restore
- häufige Sicherungen
- eventuell VM-/Container-Backups

### HostBrr

- geographisch getrennte Kopie
- clientseitig verschlüsselt
- längere Retention möglich

### Offline/Immutable

- Schutz gegen kompromittierte Zugangsdaten und Ransomware
- beispielsweise rotierende USB-Datenträger, getrenntes zweites Storage-System oder echtes immutable Object Storage

## HostBrr ersetzt nicht die Offline-Kopie

Eine per SFTP ständig erreichbare StorageBox ist zwar offsite, aber nicht automatisch offline oder immutable. Hat Malware Zugriff auf die verwendeten Credentials, kann sie möglicherweise auch die verschlüsselten Backup-Dateien löschen.

## Restore gehört zur Strategie

Ein Backup gilt erst dann als belastbar, wenn die Wiederherstellung regelmäßig geprüft wird. CISA empfiehlt ausdrücklich regelmäßige Tests der Verfügbarkeit und Integrität von Backups.

## Weiterführende Dokumentation

- CISA #StopRansomware Guide: https://www.cisa.gov/stopransomware/ransomware-guide
- CISA LockBit Advisory / 3-2-1: https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-165a
