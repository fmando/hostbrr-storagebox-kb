---
title: WebDAV
category: anwendungen
status: community-reported
last_reviewed: 2026-08-24
---
# WebDAV auf der HostBrr StorageBox

WebDAV ist interessant, wenn Clients die StorageBox über HTTPS wie einen entfernten Dateispeicher ansprechen sollen.

## Variante 1: Nextcloud als WebDAV-Schicht

In HostBrr-Community-Berichten wird **Softaculous → Nextcloud** als üblicher Weg zu WebDAV genannt.

Nextcloud WebDAV-Dokumentation: https://docs.nextcloud.com/server/latest/developer_manual/client_apis/WebDAV/index.html

Vorteile:

- HTTPS
- Benutzeroberfläche
- Benutzerverwaltung
- Desktop-/Mobile-Clients
- WebDAV-API

Nachteile:

- PHP- und Datenbank-Overhead
- mehr Komponenten als bei SFTP
- abhängig von HostBrr-PHP-Konfiguration

Community-Quelle: https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p18

## Variante 2: SFTP statt WebDAV

Wenn keine Webanwendung benötigt wird, ist SFTP häufig einfacher:

- weniger Serverkomponenten
- SSH-Key-Authentifizierung
- rclone-Unterstützung
- rsync/Restic/Borg-Workflows möglich

Für reine Backups ist SFTP daher meistens vorzuziehen.

## Native WebDAV-Unterstützung

In älteren Community-Diskussionen wurde nach Apache/LiteSpeed DAV-Modulen gefragt. Ob HostBrr heute natives WebDAV unabhängig von Nextcloud anbietet, ist derzeit **nicht verifiziert**.

Daher nicht einfach davon ausgehen, dass `https://host/path` ohne zusätzliche Anwendung WebDAV bereitstellt.

## Performance

Community-Messungen zeigen stark unterschiedliche WebDAV-Geschwindigkeiten. In einem Februar-2025-Thread wurden beispielsweise Werte von etwa 7 MB/s bis 40 MB/s genannt. Solche Zahlen hängen stark von Standort, Latenz, Client und Konfiguration ab und sind keine zugesicherte HostBrr-Leistung.

Quelle: https://lowendtalk.com/discussion/202755/hostbrr-valentine-offers-10-gbps-storagebox-7-year-cpanel-85-off-flash/p4

## Empfehlung

| Zweck | Empfehlung |
|---|---|
| Backup | SFTP + Restic/rclone/rsync |
| einfacher Remote-Dateizugriff | SFTP oder rclone |
| Browser-Cloud | Nextcloud |
| WebDAV für Apps | Nextcloud-WebDAV |
| eigener WebDAV-Daemon | eher VPS |
