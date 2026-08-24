---
title: Backup mit rsync
category: backup
status: community-reported
last_reviewed: 2026-08-24
---

# Backup mit rsync

`rsync` ist für die HostBrr StorageBox besonders interessant, weil HostBrr rsync/SSH als Teil des StorageBox-Angebots nennt und es in der Community für automatisierte Backups eingesetzt wird.

## Zwei mögliche Richtungen

### Push: Server → StorageBox

Der zu sichernde Server startet die Verbindung:

```bash
rsync -aH --info=progress2 /srv/daten/ hostbrr-storage:backup/server1/
```

Das ist konzeptionell einfach, bedeutet aber, dass der Quellserver Zugangsdaten bzw. einen Schlüssel mit Schreibzugriff auf die StorageBox besitzt.

### Pull: StorageBox → Server

In Community-Berichten wird auch der umgekehrte Weg beschrieben: Ein Cronjob auf der StorageBox zieht Daten per SSH/rsync vom Quellserver. Das kann die Auswirkungen kompromittierter Zugangsdaten anders verteilen und ist für bestimmte Backup-Designs interessant.

Das genaue Kommando hängt vom HostBrr-Shell-/Cron-Umfang und den Pfaden ab und wird vor Kennzeichnung als `verified` auf einer aktuellen Box getestet.

## Wichtiger Hinweis zu `--delete`

Ein Kommando wie

```bash
rsync -a --delete /quelle/ hostbrr-storage:backup/
```

ist **eine Synchronisation und noch keine belastbare Backup-Historie**. Wird eine Datei lokal versehentlich gelöscht oder beschädigt, kann dieser Zustand beim nächsten Lauf auf das Ziel übertragen werden.

Für echte Versionierung sind beispielsweise Restic oder Borg beziehungsweise eine zusätzliche Snapshot-/Generationenstrategie geeigneter.

## Automatisierung

DirectAdmin unterstützt Cronjobs, sofern die Funktion für den Account freigeschaltet ist. Für unbeaufsichtigte Jobs sollte SSH-Key-Authentifizierung verwendet werden; private Schlüssel dürfen nicht im Repository oder in Cron-Kommandos mit Klartext-Secrets landen.

## Restore gehört zum Howto

Ein Backup gilt erst dann als praktisch brauchbar, wenn der Rückweg dokumentiert ist. Beispiel:

```bash
rsync -aH hostbrr-storage:backup/server1/ /srv/restore-test/
```

Anschließend sollten Dateianzahl, Größen und bei wichtigen Daten Checksums geprüft werden.

## Weiterführende Dokumentation

- [rsync – offizielle Projektseite](https://rsync.samba.org/)
- [rsync Manual](https://download.samba.org/pub/rsync/rsync.1)
- [DirectAdmin Docs – Managing server over SSH](https://docs.directadmin.com/operation-system-level/os-general/managing-with-ssh.html)
- [DirectAdmin Docs – Cronjobs](https://docs.directadmin.com/webservices/cronjobs.html)

## HostBrr-spezifischer Status

| Punkt | Status |
|---|---|
| SSH/SFTP | offiziell angeboten |
| rsync | offiziell angeboten |
| rsync per SSH | Community-erprobt |
| Cron auf StorageBox | Community-erprobt, Account-Freigabe prüfen |
| Restore | noch selbst zu testen |
