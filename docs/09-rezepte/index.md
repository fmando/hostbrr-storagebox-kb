---
title: Rezepte & Howtos
category: rezepte
status: maintained
last_reviewed: 2026-08-24
---
# Rezepte & Howtos

Dieser Bereich startet nicht beim Werkzeug, sondern beim **Ziel**. Für die technische Auswahl hilft zusätzlich [Welche Backup-Methode ist die richtige?](../03-backup/welche-backup-methode.md).

## Server & Virtualisierung

| Ziel | Rezept | Ansatz |
|---|---|---|
| einzelnen VPS versioniert sichern | [VPS mit Restic](vps-restic.md) | Restic direkt über SFTP |
| mehrere VPS sichern | [Mehrere VPS zentral](mehrere-vps-zentral-sichern.md) | getrennte Repositories oder zentraler Backupserver |
| Proxmox-Archive offsite speichern | [Proxmox vzdump + rclone crypt](proxmox-vzdump-rclone-crypt.md) | lokal erzeugen, clientseitig verschlüsselt übertragen |

## NAS & Clients

| Ziel | Rezept | Ansatz |
|---|---|---|
| Synology offsite sichern | [Synology](synology.md) | Hyper Backup/rsync-kompatibler Weg |
| QNAP offsite sichern | [QNAP](qnap.md) | HBS 3/rsync |
| Windows-PC sichern | [Windows-PC](windows-pc-sichern.md) | versioniertes verschlüsseltes Backup |

## Nextcloud

| Ziel | Rezept | Wann sinnvoll? |
|---|---|---|
| möglichst einfache Installation | [Nextcloud direkt auf HostBrr](nextcloud-direkt.md) | wenn Shared-Hosting-Limits genügen |
| Anwendung und Storage trennen | [Nextcloud auf VPS + StorageBox](nextcloud-vps-storagebox.md) | mehr Kontrolle über PHP, DB und Anwendung |

Siehe auch [Nextcloud-Grundlagen](../05-anwendungen/nextcloud.md), [WebDAV](../05-anwendungen/webdav.md) und [Nextcloud-Troubleshooting](../08-troubleshooting/nextcloud.md).

## Datenzugriff & Migration

| Ziel | Rezept |
|---|---|
| StorageBox als Laufwerk mit lokalem Cache | [Cloud-Laufwerk mit Cache](cloud-drive-cache.md) |
| mehrere TB erstmals hochladen | [Große Datenmengen übertragen](initiale-datenuebertragung.md) |

Für dauerhafte Mounts zusätzlich [Linux-Mounts](../04-mounts/linux.md), [Windows SSHFS](../04-mounts/windows-sshfs.md) und [Transfers & Mounts – Troubleshooting](../08-troubleshooting/performance-mounts.md) lesen.

## Wiederherstellung

**[Disaster Recovery – Server komplett verloren](disaster-recovery.md)** ist der zentrale Einstieg für den Ernstfall.

Ein Backupkonzept ist erst vollständig, wenn der Rückweg dokumentiert und getestet ist. Ergänzend: [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md) und [Ransomware & Löschschutz](../06-sicherheit/ransomware-loeschschutz.md).

## Welche Rezepte fehlen noch für v1.0?

Die wichtigsten Grundszenarien sind abgedeckt. Vor v1.0 sollten vorhandene Rezepte vor allem um vollständige Restore-Beispiele, konsistente Sicherheitsverweise und HostBrr-spezifisch verifizierte Werte ergänzt werden. Praktische Tests an realen Boxen folgen später.

## Qualitätsstandard für jedes Rezept

Ein Rezept soll möglichst enthalten:

1. Ziel und Voraussetzungen
2. empfohlene Architektur
3. Einrichtung
4. Automatisierung
5. Verschlüsselung und Credentials
6. Restore bzw. Rückweg
7. typische Fehler
8. offizielle Dokumentation
9. HostBrr-spezifische Hinweise und deren Evidenzstatus

So bleibt ein Howto auch dann brauchbar, wenn einzelne Tool-Versionen oder HostBrr-Details später geändert werden.
