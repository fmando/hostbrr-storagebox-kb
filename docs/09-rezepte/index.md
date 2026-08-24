---
title: Rezepte & Howtos
category: rezepte
status: maintained
last_reviewed: 2026-08-24
---
# Rezepte & Howtos

Dieser Bereich startet nicht beim Werkzeug, sondern beim **Ziel**.

Wer einfach einen VPS, Proxmox-Host oder Windows-PC sichern möchte, soll nicht zuerst entscheiden müssen, ob rsync, rclone, Restic oder Borg das richtige Werkzeug ist.

## Rezepte

| Ziel | Empfohlener Weg |
|---|---|
| VPS verschlüsselt und versioniert sichern | Restic direkt über SFTP |
| Proxmox-vzdump offsite sichern | lokales vzdump + rclone crypt |
| Windows-Dateien verschlüsselt offsite kopieren | rclone + crypt über SFTP |
| StorageBox als Linux-Laufwerk | rclone mount oder SSHFS |
| Große Erstübertragung | rclone/rsync, danach inkrementell |
| Cloud-Laufwerk mit Cache | rclone mount; für viele Metadaten ggf. JuiceFS prüfen |
| Nextcloud bereitstellen | direkt via DirectAdmin/Softaculous oder Anwendung auf VPS + StorageBox passend anbinden |

## Grundregel

Ein Rezept dokumentiert immer:

1. Ziel und Voraussetzungen
2. empfohlene Architektur
3. Einrichtung
4. Automatisierung
5. Verschlüsselung
6. Restore bzw. Rückweg
7. typische Fehler
8. offizielle Dokumentation
9. HostBrr-spezifische Hinweise

So bleibt ein Howto auch dann brauchbar, wenn einzelne Tool-Versionen oder HostBrr-Details später geändert werden.
