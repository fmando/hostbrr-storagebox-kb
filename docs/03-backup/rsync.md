---
title: Backup mit rsync
category: backup
status: community-reported
last_reviewed: 2026-08-25
---
# Backup mit rsync

`rsync` ist für HostBrr besonders interessant, weil SSH/rsync offiziell zum StorageBox-Zugangsmodell gehört. Es ist schnell, transparent und hervorragend für Dateiübertragung – aber **rsync allein ist nicht automatisch ein versioniertes Backup-System**.

## Architektur

```text
Server
  ↓ rsync über SSH
HostBrr StorageBox
```

## Voraussetzungen

- rsync lokal
- rsync auf der StorageBox erreichbar
- SSH-Zugang
- vorzugsweise SSH-Key
- festgelegter Zielpfad

Zuerst prüfen:

```bash
ssh hostbrr-storage 'rsync --version'
```

Client- und Serverversion dokumentieren.

## Push: Server → StorageBox

```bash
rsync -aH --info=progress2 \
  /srv/daten/ \
  hostbrr-storage:backup/server1/current/
```

Das ist einfach und gut für Spiegel-/Kopieraufgaben. Der Quellserver besitzt dabei Schreibzugriff auf das Backupziel.

## Pull: StorageBox → Server

Wenn HostBrr-Cron und Shellumfang es erlauben, kann die StorageBox Daten auch vom Quellserver ziehen. Dieses Modell ist sicherheitstechnisch interessant, muss aber auf der aktuellen Box praktisch geprüft werden.

## `--delete` mit Vorsicht

```bash
rsync -a --delete /quelle/ hostbrr-storage:backup/current/
```

spiegelt auch Löschungen. Das ist Synchronisation, keine historische Sicherung. Ein lokaler Bedienfehler oder kompromittierter Datenbestand kann dadurch auf das Ziel übertragen werden.

Vor riskanten Änderungen zuerst:

```bash
rsync -a --delete --dry-run /quelle/ hostbrr-storage:backup/current/
```

## Einfache Generationen mit `--backup-dir`

rsync kann ersetzte oder gelöschte Dateien in ein separates Verzeichnis verschieben:

```bash
STAMP=$(date +%F-%H%M)
rsync -a --delete \
  --backup \
  --backup-dir="../versions/$STAMP" \
  /srv/daten/ \
  hostbrr-storage:backup/server1/current/
```

Damit entsteht eine einfache Historie. Sie ersetzt aber nicht automatisch die komfortablen Snapshot-, Retention- und Integritätsfunktionen von Restic/Kopia/Borg.

## `--link-dest`

rsync kann unveränderte Dateien über Hardlinks aus einer vorherigen Generation übernehmen. Das ermöglicht klassische Snapshot-Verzeichnisbäume mit geringem Mehrverbrauch.

Ob dieses Modell auf HostBrr für große Backups sinnvoll und vollständig kompatibel ist, muss praktisch getestet werden – insbesondere Hardlink-Verhalten, Quota-Abrechnung und Performance bei sehr vielen Dateien.

## Integrität

rsync verifiziert übertragene Dateien während des Transfers. Für einen späteren Audit eines bereits vorhandenen Datenbestands kann ein checksum-basierter Vergleich sinnvoll sein:

```bash
rsync -aHnc /srv/daten/ hostbrr-storage:backup/server1/current/
```

`-c/--checksum` kann bei großen Datenbeständen teuer sein, da beide Seiten Dateiinhalte lesen müssen. Nicht bei jedem täglichen Lauf blind aktivieren.

## Restore

```bash
mkdir -p /srv/restore-test
rsync -aH \
  hostbrr-storage:backup/server1/current/ \
  /srv/restore-test/
```

Anschließend Anwendung, Dateirechte und bei kritischen Daten zusätzliche Checks prüfen.

## Automatisierung

Cron oder systemd timer eignen sich gut. Ein produktiver Job braucht mindestens:

- Logging
- Exit-Code-Auswertung
- Fehlerbenachrichtigung
- Schutz vor parallelen Läufen
- gelegentlichen Restore-Test

DirectAdmin zeigt Cronjobs auf User-Ebene unter **Advanced Features → Cron Jobs**. Eine aktuelle offizielle DirectAdmin-Referenz dazu ist beispielsweise die [Changelog-Dokumentation zu Cronjob-Verbesserungen](https://docs.directadmin.com/changelog/version-1.649.html).

## Sicherheit

rsync über SSH schützt den Transport, verschlüsselt die Dateien aber **nicht clientseitig auf dem Ziel**. Wer verschlüsselte Daten auf der StorageBox benötigt, sollte beispielsweise Restic/Kopia/Borg oder rclone crypt einsetzen.

Ein kompromittierter Push-Server mit Schreib-/Löschrechten kann auch das rsync-Ziel verändern. Generationen helfen gegen Bedienfehler, ersetzen aber keine unabhängige Kopie.

## Typische Fehler

### `rsync: command not found`

Serverseitige Verfügbarkeit und PATH prüfen:

```bash
ssh hostbrr-storage 'command -v rsync; rsync --version'
```

### Permission denied

Zielpfad, SSH-Key und Schreibrechte prüfen. Nicht automatisch mit großzügigeren Dateirechten lösen.

### `--link-dest` spart keinen Platz

Hardlinks, Attribute, Pfade und HostBrr-Dateisystemverhalten prüfen. Erst nach Praxistest als empfohlene HostBrr-Strategie verwenden.

## HostBrr-spezifisch noch zu testen

- installierte rsync-Version 2 TB vs. 8 TB
- Push und Pull
- Cron auf der StorageBox
- `--backup-dir`
- `--link-dest` und Hardlinks
- Quota-Verhalten von Hardlinks
- `--checksum` bei großen Datenmengen
- Restore mit Rechten/Zeitstempeln
- 10.000+ kleine Dateien

## Einordnung

**Sehr gut für:** transparente Kopien, Spiegel, einfache Server-Transfers und selbst gebaute Generationen.

**Weniger gut für:** Nutzer, die ohne eigene Skriptlogik Verschlüsselung, Deduplizierung, Snapshot-Retention und Repository-Verifikation wünschen. Dafür sind Restic oder Kopia meist einfacher.

## Primärquellen

- https://rsync.samba.org/
- https://rsync.samba.org/ftp/rsync/rsync.1.html
- https://docs.directadmin.com/operation-system-level/os-general/managing-with-ssh.html
- https://docs.directadmin.com/changelog/version-1.649.html
