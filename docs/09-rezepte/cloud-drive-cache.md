---
title: StorageBox als Cloud-Laufwerk mit Cache
category: rezepte
status: community-reported
last_reviewed: 2026-08-24
---
# StorageBox als Cloud-Laufwerk mit lokalem Cache

## Ziel

Großer günstiger Remote-Speicher der HostBrr StorageBox wird auf einem VPS oder Linux-System eingebunden. Ein lokaler SSD/NVMe-Cache soll wiederholte Zugriffe beschleunigen und Anwendungen eine dateisystemähnliche Schnittstelle geben.

Das ist **kein Backupverfahren**, sondern eine Storage-/Mount-Architektur.

## Architektur

```text
Anwendung
   ↓
/mnt/hostbrr
   ↓ rclone VFS
lokaler SSD/NVMe-Cache
   ↓ SFTP
HostBrr StorageBox
```

## Voraussetzungen

- Linux/VPS mit stabilem Netzwerk
- ausreichend lokaler SSD/NVMe-Platz
- SFTP-Zugang zur StorageBox
- rclone
- Anwendung, die mit Remote-/FUSE-Semantik zurechtkommt
- separates Backup wichtiger Daten

Offizielle Dokumentation:

- rclone mount: https://rclone.org/commands/rclone_mount/
- rclone VFS cache: https://rclone.org/commands/rclone_mount/#vfs-file-caching
- rclone SFTP: https://rclone.org/sftp/
- JuiceFS: https://juicefs.com/docs/

## Variante A – rclone mount + VFS-Cache

Beispiel:

```bash
mkdir -p /mnt/hostbrr /var/cache/rclone-hostbrr

rclone mount hostbrr: /mnt/hostbrr \
  --vfs-cache-mode full \
  --cache-dir /var/cache/rclone-hostbrr \
  --vfs-cache-max-size 50G \
  --vfs-cache-max-age 24h
```

`--vfs-cache-mode full` legt Datei-I/O im lokalen VFS-Cache ab und verbessert die Kompatibilität gegenüber einem vollständig ungecachten Mount. Cachegröße und freier lokaler Speicher müssen zusammenpassen.

## Cache-Dimensionierung

Nicht einfach `50G` übernehmen. Vorher abschätzen:

- Größe des typischen Hot-Sets
- maximale gleichzeitig bearbeitete Datei
- freier lokaler Speicher
- Verhalten bei vollem Cache
- Schreiblast

Für einen kleinen VPS kann ein zu großer Cache das Root-Dateisystem füllen und damit das gesamte System gefährden.

## Automatisierung mit systemd

Für produktiven Betrieb sollte der Mount nicht dauerhaft in einer interaktiven Shell laufen. Ein systemd-Service kann Start, Neustart und Logging übernehmen.

Vor einer konkreten Unit müssen Pfade, Benutzer und rclone-Konfigurationsort feststehen. Wichtig ist außerdem, Anwendungen erst dann zu starten, wenn der Mount tatsächlich verfügbar ist.

## Verifikation

Nach Einrichtung nicht nur `ls` testen:

1. große Datei schreiben
2. große Datei lesen
3. Datei ändern
4. Datei umbenennen
5. Datei löschen
6. System/Service neu starten
7. Cacheverhalten beobachten
8. Netzwerk kurz unterbrechen und Recovery prüfen

Zusätzlich kontrollieren:

```bash
mount | grep hostbrr
rclone about hostbrr:
df -h /var/cache/rclone-hostbrr
```

Nicht jedes Backend unterstützt `rclone about`; falls SFTP/Server dies nicht sinnvoll liefert, Quota über DirectAdmin bzw. andere HostBrr-Werkzeuge prüfen.

## Variante B – JuiceFS

Community-Berichte beschreiben HostBrr StorageBox + SFTP + JuiceFS mit lokalem NVMe-Cache als interessante Lösung bei Workloads mit vielen Metadatenoperationen.

Das ist eine **fortgeschrittene Architektur** mit zusätzlichen Komponenten und wird in dieser KB weiterhin als `community-reported` behandelt, bis wir sie selbst reproduziert haben.

## Wann ist ein Cache-Mount sinnvoll?

Gut geeignet für:

- große Medienarchive
- selten geänderte Daten
- Cloud-Drive-artigen Zugriff
- Anwendungen, deren Hot-Set in lokalen Cache passt

Mit Vorsicht bei:

- sehr vielen kleinen Dateien
- Anwendungen mit vielen Metadatenoperationen
- gleichzeitigen Schreibern
- Anwendungen, die strikte lokale POSIX-Semantik erwarten

Nicht empfohlen für:

- Datenbanken
- VM-Datastores
- native Proxmox-Backup-Server-Datastores über einen improvisierten FUSE-Layer

## Sicherheit

Ein Mount macht Remote-Daten direkt für die lokale Anwendung erreichbar. Wird die Anwendung oder der VPS kompromittiert, können je nach Berechtigungen auch Dateien auf HostBrr verändert oder gelöscht werden.

Deshalb:

- Mount-Credentials mit minimal notwendigen Rechten verwenden, soweit möglich
- Anwendung nicht unnötig als root ausführen
- separates versioniertes/offline Backup behalten
- Cache nicht als zweite Kopie betrachten

## Restore / Recovery

Bei einem Mount gibt es keinen klassischen „Restore“, solange HostBrr die Primärdaten enthält. Für einen Ausfall des VPS sollte aber dokumentiert sein:

1. rclone-Konfiguration/Secrets aus sicherer Quelle wiederherstellen
2. neuen Cache leer anlegen
3. Mount erneut verbinden
4. Datenbestand prüfen
5. Anwendung erst danach starten

Bei Verlust oder Beschädigung der Remote-Daten hilft der Cache nicht zuverlässig; dafür ist das separate Backup zuständig.

## Typische Fehler

### Mount ist da, Anwendung meldet aber I/O-Fehler

VFS-Modus, Dateisperren/Anwendungsanforderungen, SFTP-Verbindung und Logs prüfen.

### Lokale SSD läuft voll

Cachegrenzen und freien Speicher prüfen. Ein Cache muss überwacht werden.

### Viele kleine Dateien sind langsam

Das kann durch SFTP-Latenz und Metadatenoperationen verursacht werden. Mehr Bandbreite allein löst dieses Problem nicht zwingend.

### Nach Netzwerkabbruch hängt die Anwendung

Mount-Recovery und Verhalten der Anwendung separat testen. Kritische Dienste sollten nicht voraussetzen, dass ein Remote-FUSE-Mount immer lokale Storage-Semantik besitzt.

## HostBrr-spezifisch noch zu testen

- rclone-mount-Langzeittest
- Cache 10/50/100 GB
- große vs. kleine Dateien
- Metadatenoperationen
- Verbindungsabbruch/Recovery
- parallele Zugriffe
- 2-TB- vs. 8-TB-Box
- JuiceFS-Vergleich als separater fortgeschrittener Test
