---
title: Welche Backup-Methode ist die richtige?
category: backup
status: maintained
last_reviewed: 2026-08-24
---

# Welche Backup-Methode ist die richtige?

Für eine HostBrr StorageBox gibt es nicht *die eine* beste Backup-Methode. Die richtige Wahl hängt davon ab, ob einfache Dateikopien, verschlüsselte versionierte Backups, Deduplizierung oder maximale Portabilität wichtiger sind.

Diese Seite ist die Entscheidungshilfe zwischen **rsync**, **rclone + crypt**, **Restic** und **BorgBackup**.

> **HostBrr-spezifisch:** SSH/SFTP und rsync sind die natürliche Basis der StorageBox. Ob zusätzliche Programme wie Borg serverseitig verfügbar sind, muss auf der konkreten StorageBox geprüft werden.

## Kurzentscheidung

| Anforderung | Empfehlung |
|---|---|
| Einfaches Spiegeln von Dateien | **rsync** |
| Dateien verschlüsselt auf die StorageBox kopieren | **rclone + crypt** |
| Verschlüsseltes, versioniertes Backup über SFTP | **Restic** |
| Deduplizierung + Kompression + Verschlüsselung | **Borg**, wenn serverseitig kompatibel |
| Möglichst wenig Abhängigkeit von HostBrr-Software | **Restic** oder **rclone** |
| Dateien auf der StorageBox direkt lesbar halten | **rsync** / rclone ohne crypt |
| Schutz der Inhalte vor dem Storage-Anbieter | **Restic**, **Borg** oder **rclone crypt** |
| Proxmox-vzdump offsite kopieren | **rclone crypt** oder **Restic** |

## Funktionsvergleich

| Eigenschaft | rsync | rclone + crypt | Restic | Borg |
|---|---:|---:|---:|---:|
| SFTP/SSH | Ja | Ja | Ja | Ja |
| Clientseitige Verschlüsselung | Nein¹ | Ja | Ja | Ja |
| Versionierte Snapshots | Nein² | Nein² | Ja | Ja |
| Deduplizierung | eingeschränkt³ | Nein | Ja | Ja |
| Kompression | Nein | Nein | repositoryabhängig / nicht Hauptmerkmal | Ja |
| Einzeldateien direkt auf Ziel sichtbar | Ja | verschlüsselte Objekte | Nein | Nein |
| Serverprogramm auf HostBrr nötig | rsync | Nein | Nein bei SFTP | für optimalen SSH-Betrieb Ja |
| Einfacher Restore einzelner Dateien | Sehr einfach | Einfach | Einfach | Einfach |
| Geeignet für automatisierte Backups | Ja | Ja | Ja | Ja |

¹ Transport über SSH ist verschlüsselt, die Zieldateien selbst liegen aber unverschlüsselt vor.  
² Versionierung kann mit zusätzlichen Verzeichnis-/Snapshot-Konzepten gebaut werden, gehört aber nicht zum einfachen Sync-Modell.  
³ rsync überträgt effizient nur Änderungen, ist aber kein deduplizierendes Backup-Repository wie Restic oder Borg.

## 1. rsync – wenn Einfachheit zählt

[rsync](https://rsync.samba.org/) ist ideal, wenn auf der StorageBox eine nachvollziehbare Dateikopie liegen soll.

### Vorteile

- sehr etabliert
- leicht zu automatisieren
- effiziente inkrementelle Übertragung
- Restore ohne Spezialsoftware möglich
- Zielstruktur bleibt direkt lesbar

### Nachteile

- keine eingebaute clientseitige Verschlüsselung der gespeicherten Dateien
- keine echte Snapshot-Historie im Grundbetrieb
- `--delete` kann Fehler oder versehentlich gelöschte Dateien auf das Ziel übertragen

**Geeignet für:** Website-Dateien, zusätzliche Kopien, Mirror-Jobs und einfache VPS-Backups.

Siehe [rsync-Howto](rsync.md).

## 2. rclone + crypt – flexibel und transparent verschlüsselt

[rclone](https://rclone.org/) unterstützt SFTP als Backend und kann mit dem [crypt-Backend](https://rclone.org/crypt/) eine zusätzliche verschlüsselte Sicht auf dieses Backend legen.

Die allgemeine rclone-Dokumentation empfiehlt beim Kennenlernen den interaktiven Modus, um versehentlichen Datenverlust durch Sync-Operationen zu vermeiden.

### Vorteile

- HostBrr benötigt nur SFTP
- clientseitige Verschlüsselung
- Dateinamen können ebenfalls verschlüsselt werden
- sehr flexibel für Copy, Sync und Mount
- gut für große fertige Backup-Dateien wie Proxmox `vzdump`

### Nachteile

- kein klassisches Snapshot-Backupformat
- `sync` spiegelt Löschungen und muss bewusst eingesetzt werden
- Restore benötigt rclone und die crypt-Konfiguration/Passwörter

**Geeignet für:** verschlüsselte Offsite-Kopien, Proxmox-vzdump-Dateien, Archive und bestehende Backup-Dateien.

Siehe [rclone + SFTP + crypt](rclone-sftp-crypt.md).

## 3. Restic – starke Standardempfehlung für echte Backups

[Restic](https://restic.net/) speichert mehrere Revisionen in einem verschlüsselten Repository und unterstützt [SFTP-Repositories](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#sftp) direkt.

Für HostBrr ist das besonders interessant: **Auf dem StorageBox-Server muss Restic für das SFTP-Backend nicht installiert sein.** Ein funktionierender SSH/SFTP-Zugang genügt.

### Vorteile

- clientseitige Verschlüsselung
- Snapshots und Versionierung
- Deduplizierung
- direktes SFTP-Backend
- `restic check` zur Repository-Prüfung
- Retention über `forget`

### Nachteile

- Repository ist nicht als normale Dateikopie lesbar
- Passwort/Keys sind kritisch für den Restore
- `prune` kann bei großen Remote-Repositories aufwendig sein

Restic weist darauf hin, dass `forget` Snapshots entfernt und `prune` anschließend nicht mehr benötigte Daten freigibt. Pruning kann zeitaufwendig sein und sperrt das Repository während des Vorgangs.

**Geeignet für:** Serverdaten, Home-Verzeichnisse, Konfigurationen und langfristige versionierte Offsite-Backups.

Siehe [Restic-Howto](restic.md).

## 4. BorgBackup – sehr effizient, aber HostBrr-Abhängigkeit prüfen

[BorgBackup](https://www.borgbackup.org/) kombiniert content-defined Deduplizierung, Kompression und authentifizierte Verschlüsselung. Borg kann Remote-Repositories über SSH betreiben und profitiert deutlich davon, wenn Borg auch auf dem Remote-Host installiert ist.

### Vorteile

- sehr gute Deduplizierung
- Kompression
- clientseitige authentifizierte Verschlüsselung
- Snapshots/Archive
- `borg check`, `prune` und `compact`
- Archive können für Restore-Zwecke gemountet werden

### Nachteile für HostBrr

- optimale Remote-Nutzung hängt von einer kompatiblen Borg-Installation auf HostBrr ab
- Borg-Versionen zwischen Client und Server müssen berücksichtigt werden
- ein über SSHFS gemountetes Remote-Dateisystem als Ersatz ist eine zusätzliche Fehler-/Performance-Schicht

Borg selbst weist darauf hin, dass Remote-Betrieb besonders effizient ist, wenn Borg auf dem Zielhost installiert ist. Die Borg-Dokumentation warnt außerdem davor, beliebige Netzwerkdateisysteme ungeprüft als Backup-Unterbau zu verwenden.

**Geeignet für:** häufige Backups großer, sich teilweise ändernder Datenbestände, wenn die aktuelle HostBrr-Box Borg zuverlässig unterstützt.

Siehe [BorgBackup-Howto](borg.md).

## Entscheidung nach Szenario

### VPS vollständig sichern

**Bevorzugt: Restic.**

Warum: versioniert, verschlüsselt, dedupliziert und nur SFTP auf HostBrr erforderlich.

### Proxmox-VM-/LXC-Backups

Wenn Proxmox bereits `vzdump`-Archive erzeugt:

**Bevorzugt: rclone crypt** für die Offsite-Kopie der fertigen Archive.

Alternativ kann Restic die Backup-Dateien in ein versioniertes Repository aufnehmen. Bei großen VM-Images sollte der zusätzliche Repository-Overhead gegen den Nutzen der Deduplizierung getestet werden.

### Website sichern

Für eine unmittelbar lesbare Kopie:

**rsync** für Dateien + separater Datenbank-Dump.

Für echte Historie und Verschlüsselung:

**Restic**.

### Persönliche Dokumente

**Restic** als Standardempfehlung. Bei Bedarf an einer verschlüsselten, dateibasierten Ablage ist **rclone crypt** ebenfalls attraktiv.

### Große Medienarchive

Wenn Dateien überwiegend unverändert bleiben und bereits eine lokale Primärkopie existiert:

**rclone crypt** ist einfach und nachvollziehbar.

Bei vielen ähnlichen oder sich ändernden Dateien kann **Borg** durch Deduplizierung interessant werden, sofern HostBrr-Kompatibilität verifiziert ist.

## Was wir für HostBrr aktuell bevorzugen

Für die KB verwenden wir vorläufig folgende Priorisierung:

1. **Restic** – beste Kombination aus echtem Backup, Verschlüsselung und geringer HostBrr-Abhängigkeit.
2. **rclone + crypt** – ideal für verschlüsselte Kopien bereits erzeugter Backup-Dateien.
3. **rsync** – ideal für einfache, direkt lesbare Spiegelungen.
4. **Borg** – technisch sehr attraktiv, aber aktuelle serverseitige HostBrr-Kompatibilität zuerst prüfen.

Diese Reihenfolge ist **keine allgemeine Rangliste der Programme**, sondern eine Einschätzung speziell für das StorageBox-Modell.

## Sicherheitsregel: Sync ist nicht automatisch Backup

Ein synchronisierter Mirror kann versehentliche Löschungen, beschädigte Dateien oder Ransomware-Veränderungen ebenfalls übernehmen. Für wichtige Daten sollte mindestens eine **versionierte und vom Quellsystem getrennte Wiederherstellungsmöglichkeit** existieren.

Unabhängig vom Werkzeug gilt: Ein Backup ist erst dann belastbar, wenn ein Restore praktisch funktioniert hat.

## Weiterführende Dokumentation

- [rsync – offizielle Projektseite](https://rsync.samba.org/)
- [rclone – Dokumentation](https://rclone.org/docs/)
- [rclone SFTP](https://rclone.org/sftp/)
- [rclone crypt](https://rclone.org/crypt/)
- [Restic – Dokumentation](https://restic.readthedocs.io/)
- [Restic SFTP Repository](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#sftp)
- [Restic Forget/Prune](https://restic.readthedocs.io/en/stable/060_forget.html)
- [BorgBackup – Projektseite](https://www.borgbackup.org/)
- [BorgBackup – stabile Dokumentation](https://borgbackup.readthedocs.io/en/stable/)
