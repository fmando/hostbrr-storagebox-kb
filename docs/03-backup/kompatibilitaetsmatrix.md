---
title: Backup-Kompatibilitätsmatrix
category: backup
status: maintained
last_reviewed: 2026-08-25
---
# Backup-Kompatibilitätsmatrix

Diese Seite beantwortet zwei unterschiedliche Fragen:

1. **Passt das Verfahren technisch zur HostBrr StorageBox?**
2. **Ist es für den jeweiligen Backup-Zweck auch sinnvoll?**

Die eigentliche Auswahl nach Anwendungsfall steht unter [Welche Backup-Methode ist die richtige?](welche-backup-methode.md). Diese Matrix dokumentiert vor allem **Kompatibilität, Abhängigkeiten und Evidenz**.

## Status-Legende

| Status | Bedeutung |
|---|---|
| `official` | Funktion wird von HostBrr für die StorageBox offiziell angeboten oder dokumentiert |
| `community-reported` | Nutzung wurde von mehreren oder einzelnen Anwendern berichtet, aber noch nicht von uns reproduziert |
| `historical` | Aussage gilt für eine frühere Box-/Softwaregeneration und muss aktuell erneut geprüft werden |
| `technical-fit` | funktioniert konzeptionell mit vorhandenen Standardprotokollen, auch wenn kein HostBrr-Praxisbeleg vorliegt |
| `not-recommended` | technisch eventuell möglich, für diesen Zweck aber unnötig riskant oder architektonisch ungeeignet |

## Backup-Werkzeuge

| Werkzeug | HostBrr-Seite benötigt | Verschlüsselung ruhender Daten | Historie/Snapshots | HostBrr-Evidenz | Rolle in dieser KB |
|---|---|---:|---:|---|---|
| **Restic** | SFTP | Ja | Ja | community-reported / technical-fit | **Standardempfehlung für echte Server-Backups** |
| **Kopia** | SFTP | Ja | Ja | community-reported | starke Alternative, besonders für Policies/GUI |
| **Borg** | SSH + kompatibles `borg serve` | Ja | Ja | historical + community-reported | technisch stark, aktuelle Serverversion zuerst prüfen |
| **rclone + crypt** | SFTP | Ja | Nein | community-reported mehrfach | **verschlüsselte Offsite-Kopie fertiger Dateien/Archive** |
| **rsync** | SSH/rsync | Nein¹ | Nein² | official + community-reported | **transparente Kopien, Mirrors, eigene Generationenmodelle** |

¹ SSH verschlüsselt den Transport, nicht automatisch die auf der StorageBox liegenden Dateien.  
² Mit `--backup-dir`, `--link-dest` oder getrennten Zielverzeichnissen kann Versionierung gebaut werden; sie ist aber nicht Teil des einfachen rsync-Mirror-Modells.

## Zugriff und Mounts

| Verfahren | Einschätzung | Verwendung |
|---|---|---|
| **SSHFS** | geeignet | einfacher Linux-/Windows-Dateizugriff; kein Backupformat |
| **rclone mount + VFS Cache** | geeignet | Cloud-Drive-artiger Zugriff; Latenz und Cache beachten |
| **JuiceFS über SFTP** | community-reported / fortgeschritten | interessant bei vielen Metadatenoperationen; später reproduzieren |

Ein Mount ersetzt kein Backup. Ein kompromittierter Client kann bei Schreibrechten auch die entfernten Daten verändern oder löschen.

## Proxmox

| Ansatz | Einschätzung | Empfehlung |
|---|---|---|
| `vzdump` lokal → rclone crypt → StorageBox | sehr gut passend | **bevorzugter einfacher Offsite-Weg** |
| `vzdump` lokal → Restic/Kopia | gut passend | sinnvoll, wenn Snapshot-Historie auf Repository-Ebene gewünscht ist |
| `vzdump` lokal → rsync | gut passend | einfach und transparent, zusätzliche Generationenstrategie nötig |
| PBS-Datastore direkt auf SSHFS/rclone-FUSE | **not-recommended** | zusätzliche Dateisystem-/WAN-Schicht; nicht als Standardarchitektur verwenden |

Siehe [Proxmox-Rezept](../09-rezepte/proxmox-vzdump-rclone-crypt.md).

## Entscheidung nach Zweck

| Zweck | Erste Wahl | Gute Alternative |
|---|---|---|
| einzelner Linux-VPS | Restic | Kopia |
| mehrere VPS | Restic/Kopia pro System oder zentraler Backuphost | Borg nach Verifikation |
| Proxmox-vzdump offsite | rclone crypt | Restic/Kopia |
| Windows-Dateien versioniert | Restic | Kopia |
| NAS mit nativen Herstellerwerkzeugen | Hyper Backup/HBS über rsync, falls kompatibel | Restic/rclone in eigener Laufzeitumgebung |
| großes Medien-/Archiv-Repository | rclone crypt | rsync bei unkritischen Klartextdaten |
| direkt lesbarer Mirror | rsync | rclone ohne crypt |
| maximale HostBrr-Unabhängigkeit | Restic/Kopia/rclone | rsync |
| hohe Deduplizierung + Kompression | Borg nach Kompatibilitätsprüfung | Restic/Kopia |

## HostBrr-spezifische Abhängigkeit

Ein wesentlicher Unterschied liegt darin, **was auf dem StorageBox-Server vorhanden sein muss**:

```text
Restic ─┐
Kopia  ─┼─ brauchen nur SFTP
rclone ─┘

rsync ───── braucht rsync/SSH auf dem Ziel

Borg ────── profitiert bzw. benötigt für das normale Remote-Modell
            eine kompatible Borg-Installation auf dem Ziel
```

Damit sind Restic, Kopia und rclone besonders portabel gegenüber Änderungen der serverseitig installierten Zusatzprogramme.

## Was noch nicht als `verified` gilt

Bis zur späteren Praxisphase bleiben insbesondere offen:

- aktuelle SSH-/SFTP-Parameter beider Boxen
- Kopia über SFTP auf aktueller Generation
- aktuelle Borg-Version und Client-/Server-Kompatibilität
- rsync `--link-dest` und Hardlinks/Quota-Verhalten
- optimale Parallelität für rclone/Restic/Kopia
- Integritätsprüfung und vollständiger Restore aller Verfahren
- Unterschiede zwischen 2-TB- und 8-TB-Box

## Priorität der späteren Tests

1. Basiszugang: SSH/SFTP/rsync
2. Restic: Backup → `check` → Restore
3. rclone crypt: Copy → `cryptcheck` → Restore
4. Kopia: Snapshot → Verify → Restore
5. Borg: Versionen → Repository → Check → Restore
6. rsync: Mirror + Generationenmodell + Restore
7. gleiche Datensätze auf 2-TB- und 8-TB-Box vergleichen

## Quellen und Vertiefung

HostBrr- und Community-Fundstellen werden im Quellenregister gesammelt. Für die konkrete Einrichtung immer zusätzlich die jeweilige Primärdokumentation verwenden:

- HostBrr StorageBox: https://hostbrr.com/storageboxes.html
- Restic: https://restic.readthedocs.io/
- Kopia: https://kopia.io/docs/
- BorgBackup: https://borgbackup.readthedocs.io/
- rclone: https://rclone.org/docs/
- rsync: https://rsync.samba.org/

Für die **Auswahl** des Werkzeugs: [Welche Backup-Methode?](welche-backup-methode.md)  
Für die **praktische Umsetzung**: [Rezepte & Howtos](../09-rezepte/index.md)
