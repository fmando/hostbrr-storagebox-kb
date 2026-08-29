---
title: HostBrr StorageBox Knowledge Base
status: maintained
last_reviewed: 2026-08-28
---

# HostBrr StorageBox Knowledge Base

Deutschsprachige, **quellenbasierte** Wissensdatenbank zur **HostBrr StorageBox** – von Einrichtung über Backup bis Disaster Recovery.

<div class="grid cards" markdown>

- :material-rocket-launch: __Neu hier? Schnellstart__

    ---

    Wähle dein Ziel, nicht das Werkzeug. In 5 Minuten zum richtigen Pfad.

    [:octicons-arrow-right-24: Zum Schnellstart](00-schnellstart.md)

- :material-shield-lock: __Backup zielgerichtet__

    ---

    Restic, Kopia, Borg, rclone & rsync im direkten Vergleich.

    [:octicons-arrow-right-24: Welche Methode?](03-backup/welche-backup-methode.md)

- :material-lifebuoy: __Server verloren?__

    ---

    Nicht überschreiben – strukturiert wiederherstellen.

    [:octicons-arrow-right-24: Disaster Recovery](09-rezepte/disaster-recovery.md)

</div>

---

## Bereiche im Überblick

<div class="grid cards" markdown>

- :material-package-variant-closed: __Grundlagen__

    ---

    Produkt, Tarife, Generationen, Limits & Fair Use.

    [:octicons-arrow-right-24: Grundlagen](01-grundlagen/index.md)

- :material-login: __Zugang & DirectAdmin__

    ---

    Ersteinrichtung, SSH/SFTP, Keys, Cron, SSL, Datenbanken.

    [:octicons-arrow-right-24: Zugang](02-zugang/index.md)

- :material-backup-restore: __Backup__

    ---

    Kompatibilitätsmatrix, rsync, rclone crypt, Borg, Restic, Kopia, Proxmox.

    [:octicons-arrow-right-24: Backup](03-backup/index.md)

- :material-harddisk: __Mounts__

    ---

    StorageBox als Laufwerk unter Linux & Windows (SSHFS).

    [:octicons-arrow-right-24: Mounts](04-mounts/index.md)

- :material-cloud: __Anwendungen__

    ---

    Nextcloud, WebDAV & was direkt auf der Box läuft.

    [:octicons-arrow-right-24: Anwendungen](05-anwendungen/index.md)

- :material-security: __Sicherheit__

    ---

    Bedrohungsmodell, Verschlüsselung, 3-2-1, Ransomware-Schutz.

    [:octicons-arrow-right-24: Sicherheit](06-sicherheit/index.md)

- :material-speedometer: __Performance__

    ---

    10 Gbit/s Realität, kleine vs. große Dateien, Routing.

    [:octicons-arrow-right-24: Performance](07-performance/index.md)

- :material-wrench: __Troubleshooting__

    ---

    SSH/SFTP, Quota, Cron, Backup-Tools, Mounts, Nextcloud.

    [:octicons-arrow-right-24: Troubleshooting](08-troubleshooting/index.md)

- :material-book-open-page-variant: __Rezepte & Howtos__

    ---

    Konkrete Anleitungen für VPS, NAS, Nextcloud & Migration.

    [:octicons-arrow-right-24: Rezepte](09-rezepte/index.md)

</div>

---

## Die wichtigsten Praxis-Howtos

<div class="grid cards" markdown>

- :material-server: __Linux-VPS sichern__

    ---

    Verschlüsselt & versioniert mit Restic über SFTP.

    [:octicons-arrow-right-24: VPS mit Restic](09-rezepte/vps-restic.md)

- :material-server-network: __Mehrere VPS zentral__

    ---

    Ein Backuphost für alle Systeme.

    [:octicons-arrow-right-24: Zentral sichern](09-rezepte/mehrere-vps-zentral-sichern.md)

- :material-microsoft-windows: __Windows-PC__

    ---

    PC-Backup direkt auf die StorageBox.

    [:octicons-arrow-right-24: Windows-PC](09-rezepte/windows-pc-sichern.md)

- :material-nas: __Synology / QNAP__

    ---

    NAS nativ via rsync/SFTP sichern.

    [:octicons-arrow-right-24: Synology](09-rezepte/synology.md) · [QNAP](09-rezepte/qnap.md)

- :material-cloud-outline: __Nextcloud__

    ---

    Direkt auf der Box oder VPS + StorageBox entkoppelt.

    [:octicons-arrow-right-24: Direkt](09-rezepte/nextcloud-direkt.md) · [VPS](09-rezepte/nextcloud-vps-storagebox.md)

- :material-restore: __Disaster Recovery__

    ---

    Kompletter Serververlust – Schritt für Schritt.

    [:octicons-arrow-right-24: Recovery](09-rezepte/disaster-recovery.md)

</div>

---

## Backup-Werkzeuge kurz

- [:material-shield-check: **Restic**](03-backup/restic.md) – Standardempfehlung für versionierte Server-Backups über SFTP.
- [:material-checkbox-marked-circle-auto-outline: **Kopia**](03-backup/kopia.md) – Snapshot-Backup mit Policies und GUI.
- [:material-database: **BorgBackup**](03-backup/borg.md) – sehr effizient, HostBrr-Kompatibilität prüfen.
- [:material-lock: **rclone + crypt**](03-backup/rclone-sftp-crypt.md) – verschlüsselte Offsite-Kopien fertiger Archive.
- [:material-sync: **rsync**](03-backup/rsync.md) – transparent, aber ohne Snapshot-Historie.

---

## DirectAdmin schnell erreichbar

[Übersicht](02-zugang/directadmin.md) · [Ersteinrichtung](02-zugang/directadmin-ersteinrichtung.md) · [File Manager](02-zugang/directadmin-filemanager-pfade.md) · [SSH-Key](02-zugang/directadmin-ssh-key.md) · [Cronjobs](02-zugang/directadmin-cronjobs.md) · [Domains & SSL](02-zugang/directadmin-domains-ssl.md) · [Datenbanken](02-zugang/directadmin-datenbanken.md) · [PHP & LiteSpeed](02-zugang/directadmin-php-litespeed.md) · [Softaculous](02-zugang/directadmin-softaculous.md) · [API](02-zugang/directadmin-api.md)

---

## Statusmodell

Die Dokumente nutzen `status` für Reifegrad & Evidenzgrad:

| Status | Bedeutung |
|---|---|
| `maintained` | redaktionell gepflegt |
| `draft` | Entwurf, unvollständig |
| `research` | Recherchephase |
| `official` | durch HostBrr/Primärquelle belegt |
| `community-reported` | Community-Bericht, ungeprüft |
| `verified` | selbst auf StorageBox getestet |
| `deprecated` | veraltet |

> Quellen liegen unter `sources/` und werden erst nach Prüfung in die Doku übernommen.
