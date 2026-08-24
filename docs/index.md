---
title: HostBrr StorageBox Knowledge Base
status: maintained
last_reviewed: 2026-08-25
---

# HostBrr StorageBox Knowledge Base

Deutschsprachige, quellenbasierte Wissensdatenbank zur **HostBrr StorageBox** – mit Grundlagen, DirectAdmin, Backup, Sicherheit, Performance, Troubleshooting und praxisorientierten Howtos.

> **Neu hier?** Beginne mit dem [Schnellstart](00-schnellstart.md).  
> **Du willst sofort ein Backup einrichten?** Öffne [Welche Backup-Methode ist die richtige?](03-backup/welche-backup-methode.md).  
> **Ein System ist bereits verloren?** Gehe direkt zu [Disaster Recovery](09-rezepte/disaster-recovery.md).

## Klickbares Inhaltsverzeichnis

| Bereich | Worum geht es? | Direkteinstieg |
|---|---|---|
| 🚀 **Schnellstart** | Ziel auswählen statt Werkzeug suchen | [Schnellstart & wichtigste Docs](00-schnellstart.md) |
| 📦 **Grundlagen** | Produkt, Tarife, Generationen, Limits | [Grundlagen](01-grundlagen/index.md) · [Produktreferenz](01-grundlagen/produktreferenz.md) · [Ressourcenlimits](01-grundlagen/ressourcenlimits.md) |
| 🔐 **Zugang & DirectAdmin** | Ersteinrichtung, SSH, Keys, Cron, SSL, Datenbanken | [DirectAdmin](02-zugang/directadmin.md) · [Ersteinrichtung](02-zugang/directadmin-ersteinrichtung.md) · [SSH/SFTP](02-zugang/ssh-sftp.md) |
| 💾 **Backup** | Restic, Kopia, Borg, rclone, rsync, Proxmox | [Backup-Übersicht](03-backup/index.md) · [Welche Methode?](03-backup/welche-backup-methode.md) · [Kompatibilitätsmatrix](03-backup/kompatibilitaetsmatrix.md) |
| 🗂️ **Mounts** | StorageBox als Laufwerk unter Linux/Windows | [Mounts](04-mounts/index.md) · [Linux](04-mounts/linux.md) · [Windows](04-mounts/windows-sshfs.md) |
| ☁️ **Anwendungen** | Nextcloud, WebDAV und sinnvolle direkte Anwendungen | [Anwendungen](05-anwendungen/index.md) · [Nextcloud](05-anwendungen/nextcloud.md) · [WebDAV](05-anwendungen/webdav.md) |
| 🛡️ **Sicherheit** | Verschlüsselung, 3-2-1, SSH-Härtung, Ransomware | [Sicherheit](06-sicherheit/index.md) · [Bedrohungsmodell](06-sicherheit/bedrohungsmodell.md) · [3-2-1](06-sicherheit/3-2-1-strategie.md) |
| ⚡ **Performance** | 10 Gbit/s, kleine Dateien, Routing, Community-Werte | [Performance](07-performance/index.md) · [10 Gbit/s realistisch](07-performance/10gbit-realitaet.md) · [Messwerte](07-performance/messwerte-community.md) |
| 🧰 **Troubleshooting** | SSH, DirectAdmin, Backuptools, Mounts, Nextcloud | [Troubleshooting](08-troubleshooting/index.md) |
| 📘 **Rezepte & Howtos** | konkrete Aufgaben von VPS bis NAS und Restore | [Rezepte](09-rezepte/index.md) |
| 🩺 **Zuverlässigkeit** | Ausfälle, Wartungen, Migrationen, Provider-Backup | [Zuverlässigkeit & Migrationen](11-zuverlaessigkeit.md) |
| ⚖️ **Rechtliches** | Terms, AUP, SLA/Haftung, DSGVO | [Rechtliches](12-rechtliches/index.md) |
| 🗺️ **Roadmap** | Reifegrad, offene Punkte und Weg zu v1.0 | [Bestandsaufnahme & Roadmap](10-bestandsaufnahme.md) |

## Die wichtigsten Praxis-Howtos

| Aufgabe | Empfohlener Einstieg |
|---|---|
| Linux-VPS verschlüsselt und versioniert sichern | [VPS mit Restic](09-rezepte/vps-restic.md) |
| mehrere VPS auf eine StorageBox sichern | [Mehrere VPS zentral sichern](09-rezepte/mehrere-vps-zentral-sichern.md) |
| Proxmox-Backups offsite speichern | [vzdump + rclone crypt](09-rezepte/proxmox-vzdump-rclone-crypt.md) |
| Windows-PC sichern | [Windows-PC sichern](09-rezepte/windows-pc-sichern.md) |
| Synology sichern | [Synology → HostBrr](09-rezepte/synology.md) |
| QNAP sichern | [QNAP → HostBrr](09-rezepte/qnap.md) |
| Nextcloud ohne zusätzlichen VPS | [Nextcloud direkt auf HostBrr](09-rezepte/nextcloud-direkt.md) |
| Nextcloud mit eigenem VPS | [Nextcloud VPS + StorageBox](09-rezepte/nextcloud-vps-storagebox.md) |
| StorageBox als Cloud-Laufwerk | [Cloud-Laufwerk mit Cache](09-rezepte/cloud-drive-cache.md) |
| mehrere TB erstmals hochladen | [Initiale Datenübertragung](09-rezepte/initiale-datenuebertragung.md) |
| kompletter Serververlust | [Disaster Recovery](09-rezepte/disaster-recovery.md) |

## Backup-Werkzeuge

- [Restic](03-backup/restic.md) – Standardempfehlung für versionierte Server-Backups über SFTP.
- [Kopia](03-backup/kopia.md) – Snapshot-Backup mit Policies und optionaler GUI.
- [BorgBackup](03-backup/borg.md) – sehr effizient, serverseitige Borg-Kompatibilität auf HostBrr beachten.
- [rclone + crypt](03-backup/rclone-sftp-crypt.md) – ideal für verschlüsselte Offsite-Kopien fertiger Archive.
- [rsync](03-backup/rsync.md) – transparent und direkt lesbar, aber im Grundbetrieb keine Snapshot-Historie.

## DirectAdmin schnell erreichbar

- [Übersicht](02-zugang/directadmin.md)
- [Ersteinrichtung](02-zugang/directadmin-ersteinrichtung.md)
- [File Manager & Pfade](02-zugang/directadmin-filemanager-pfade.md)
- [SSH-Key einrichten](02-zugang/directadmin-ssh-key.md)
- [Cronjobs](02-zugang/directadmin-cronjobs.md)
- [Domains & SSL](02-zugang/directadmin-domains-ssl.md)
- [Datenbanken](02-zugang/directadmin-datenbanken.md)
- [PHP & LiteSpeed](02-zugang/directadmin-php-litespeed.md)
- [Softaculous](02-zugang/directadmin-softaculous.md)
- [API & Automatisierung](02-zugang/directadmin-api.md)

## Qualitätsstufen

| Status | Bedeutung |
|---|---|
| `official` | durch HostBrr bzw. eine Primärquelle dokumentiert |
| `community-reported` | Community-Bericht, noch nicht selbst geprüft |
| `verified` | praktisch auf einer aktuellen StorageBox selbst getestet |
| `deprecated` | veraltet oder einer älteren Produktgeneration zuzuordnen |

Aussagen aus Foren werden nicht ungeprüft zu Fakten erklärt. Relevante Fundstellen werden unter `sources/` dokumentiert und anschließend in die eigentliche Dokumentation konsolidiert. Eigene praktische Tests folgen in einer späteren Phase.
