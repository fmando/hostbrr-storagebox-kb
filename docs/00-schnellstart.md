# Schnellstart & wichtigste Dokumente

Diese Seite ist der kurze Weg durch die Knowledge Base. Wenn du nicht weißt, wo du anfangen sollst, wähle dein **Ziel** statt eines Werkzeugs.

## Ich habe gerade eine neue StorageBox bekommen

1. [Was ist die StorageBox?](01-grundlagen/was-ist-die-storagebox.md)
2. [DirectAdmin – Ersteinrichtung](02-zugang/directadmin-ersteinrichtung.md)
3. [File Manager & tatsächliche Pfade](02-zugang/directadmin-filemanager-pfade.md)
4. [SSH-Key einrichten](02-zugang/directadmin-ssh-key.md)
5. [SSH & SFTP testen](02-zugang/ssh-sftp.md)
6. [Bedrohungsmodell verstehen](06-sicherheit/bedrohungsmodell.md)

Danach erst entscheiden, **wofür** die Box eingesetzt werden soll.

## Ich möchte ein echtes, versioniertes Backup

**Erste Anlaufstelle:** [Welche Backup-Methode ist die richtige?](03-backup/welche-backup-methode.md)

Kurzfassung:

| Ziel | Empfehlung |
|---|---|
| Linux-VPS | **Restic**; Kopia als starke Alternative |
| mehrere VPS | Restic/Kopia pro System oder zentraler Backuphost |
| Windows-PC | Restic oder Kopia |
| maximale Deduplizierung/Kompression | Borg nach HostBrr-Kompatibilitätsprüfung |

Praxis:

- [VPS verschlüsselt mit Restic sichern](09-rezepte/vps-restic.md)
- [Mehrere VPS zentral sichern](09-rezepte/mehrere-vps-zentral-sichern.md)
- [Windows-PC sichern](09-rezepte/windows-pc-sichern.md)

Die technische HostBrr-Eignung der Werkzeuge steht zusätzlich in der [Backup-Kompatibilitätsmatrix](03-backup/kompatibilitaetsmatrix.md).

## Ich habe bereits fertige Backup-Dateien oder Archive

Beispiele: Proxmox-`vzdump`, Datenbank-Dumps, exportierte Archive oder große unveränderliche Dateibestände.

**Bevorzugt:** [rclone + SFTP + crypt](03-backup/rclone-sftp-crypt.md), wenn die Offsite-Kopie auf HostBrr clientseitig verschlüsselt sein soll.

Für Proxmox:

→ [Proxmox vzdump + rclone crypt](09-rezepte/proxmox-vzdump-rclone-crypt.md)

Wenn statt einer reinen Offsite-Kopie zusätzlich Repository-Snapshots und Retention gewünscht sind, kommen Restic oder Kopia in Frage.

## Ich möchte einfach Dateien spiegeln oder direkt lesbar ablegen

→ [rsync](03-backup/rsync.md)

Das ist sinnvoll für transparente Kopien und Mirrors. Wichtig: Ein Mirror mit `--delete` ist **keine vollständige Backup-Historie**.

## Ich möchte mein NAS sichern

- [Synology NAS sichern](09-rezepte/synology.md)
- [QNAP NAS sichern](09-rezepte/qnap.md)

Bei nativen NAS-Werkzeugen ist rsync ein naheliegendes Zielprotokoll. Verschlüsselung, Versionierung und Restore müssen trotzdem bewusst konfiguriert werden.

## Ich möchte Nextcloud oder WebDAV

Es gibt zwei grundsätzlich unterschiedliche Architekturen:

- [Nextcloud direkt auf der StorageBox](09-rezepte/nextcloud-direkt.md) – einfach, aber stärker von Shared-Hosting-/PHP-Limits abhängig.
- [Nextcloud auf VPS + StorageBox](09-rezepte/nextcloud-vps-storagebox.md) – Anwendung und Massenspeicher getrennt.

Ergänzend:

- [Nextcloud Grundlagen](05-anwendungen/nextcloud.md)
- [WebDAV](05-anwendungen/webdav.md)
- [PHP & LiteSpeed](02-zugang/directadmin-php-litespeed.md)
- [Cronjobs](02-zugang/directadmin-cronjobs.md)
- [Nextcloud Troubleshooting](08-troubleshooting/nextcloud.md)

## Ich möchte die StorageBox als Laufwerk verwenden

- [Linux-Mount](04-mounts/linux.md)
- [Windows mit SSHFS](04-mounts/windows-sshfs.md)
- [Cloud-Laufwerk mit Cache](09-rezepte/cloud-drive-cache.md)
- [Transfers & Mounts – Troubleshooting](08-troubleshooting/performance-mounts.md)

Ein Mount ist **Storagezugriff, kein Backup**. Für wichtige Daten zusätzlich eine versionierte Sicherung einplanen.

## Ich muss mehrere Terabyte übertragen

1. [Große Datenmengen erstmals übertragen](09-rezepte/initiale-datenuebertragung.md)
2. [10 Gbit/s in der Praxis](07-performance/10gbit-realitaet.md)
3. [Große vs. kleine Dateien](07-performance/grosse-kleine-dateien.md)
4. [Latenz & Routing](07-performance/latenz-routing.md)
5. [Community-Messwerte](07-performance/messwerte-community.md)

## Ich möchte möglichst wenig von HostBrr-Zusatzsoftware abhängig sein

Bevorzuge Verfahren, die auf der StorageBox nur Standardzugriff brauchen:

- **Restic über SFTP**
- **Kopia über SFTP**
- **rclone über SFTP**

Borg ist technisch sehr attraktiv, hängt für das normale Remote-Modell aber stärker von einer kompatiblen Borg-Installation auf HostBrr ab.

## Ich möchte die StorageBox absichern

Empfohlene Reihenfolge:

1. [Bedrohungsmodell](06-sicherheit/bedrohungsmodell.md)
2. [SSH-Key-Härtung](06-sicherheit/ssh-key-haertung.md)
3. [Clientseitige Verschlüsselung](06-sicherheit/verschluesselung.md)
4. [3-2-1-Strategie](06-sicherheit/3-2-1-strategie.md)
5. [Ransomware & Löschschutz](06-sicherheit/ransomware-loeschschutz.md)
6. [Zuverlässigkeit, Wartung & Migrationen](11-zuverlaessigkeit.md)

> **Grundregel:** HostBrr-Storage ist eine mögliche Offsite-Kopie, aber nicht automatisch immutable und laut bisheriger Recherche nicht als zusätzliche providerseitige Backupkopie zu behandeln.

## Etwas funktioniert nicht

- [Troubleshooting-Übersicht](08-troubleshooting/index.md)
- [SSH/SFTP](08-troubleshooting/ssh-sftp.md)
- [DirectAdmin, Quota & Cron](08-troubleshooting/directadmin-quota-cron.md)
- [Backup-Tools](08-troubleshooting/backup-tools.md)
- [Transfers & Mounts](08-troubleshooting/performance-mounts.md)
- [Nextcloud](08-troubleshooting/nextcloud.md)

## Mein Server ist verloren

Nicht zuerst hektisch Daten überschreiben. Beginne mit:

→ **[Disaster Recovery – Server komplett verloren](09-rezepte/disaster-recovery.md)**

Danach das zum eingesetzten Backupverfahren passende Werkzeugkapitel verwenden.

## Ich möchte wissen, was HostBrr vertraglich zusagt

- [Rechtliches – Übersicht](12-rechtliches/index.md)
- [Terms für die StorageBox](12-rechtliches/terms-storagebox.md)
- [Acceptable Use / Fair Use](12-rechtliches/acceptable-use.md)
- [SLA & Haftung](12-rechtliches/sla-haftung.md)
- [Datenschutz & DSGVO](12-rechtliches/datenschutz-dsgvo.md)

## Die acht Kerndokumente

Wenn du nur einen kleinen Teil der KB lesen möchtest:

1. [Was ist die StorageBox?](01-grundlagen/was-ist-die-storagebox.md)
2. [DirectAdmin – Ersteinrichtung](02-zugang/directadmin-ersteinrichtung.md)
3. [Welche Backup-Methode?](03-backup/welche-backup-methode.md)
4. [Backup-Kompatibilitätsmatrix](03-backup/kompatibilitaetsmatrix.md)
5. [Bedrohungsmodell](06-sicherheit/bedrohungsmodell.md)
6. [3-2-1-Strategie](06-sicherheit/3-2-1-strategie.md)
7. [Zuverlässigkeit, Wartung & Migrationen](11-zuverlaessigkeit.md)
8. [Disaster Recovery](09-rezepte/disaster-recovery.md)

## Status der Knowledge Base

Die aktuelle Bestandsaufnahme und die offenen Punkte für Version 1.0 stehen unter [Bestandsaufnahme & Roadmap](10-bestandsaufnahme.md). Praktische Tests an realen StorageBoxen und offene Support-Fragen sind bewusst eine spätere Phase.
