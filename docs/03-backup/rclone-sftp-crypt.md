---
title: rclone über SFTP mit optionaler Verschlüsselung
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# rclone über SFTP mit optionaler Verschlüsselung

`rclone` über SFTP ist eine der am häufigsten berichteten Kombinationen für die HostBrr StorageBox. Es eignet sich besonders für Dateiübertragung, verschlüsselte Offsite-Kopien und als Baustein hinter bereits versionierten Backups.

> **Wichtig:** rclone ist primär ein Transfer-/Sync-Werkzeug. `rclone crypt` ergänzt Verschlüsselung, aber nicht automatisch Snapshot-Historie und Retention wie Restic, Borg oder Kopia.

## Architektur

```text
Quellsystem
   ↓ rclone
optional: crypt remote
   ↓
SFTP/SSH
   ↓
HostBrr StorageBox
```

## Voraussetzungen

- aktuelle rclone-Version
- HostBrr-Hostname
- Benutzername
- tatsächlicher SSH-Port
- bevorzugt SSH-Key
- eigener Zielordner
- bei `crypt`: Passwort und Salt/zweites Passwort sicher außerhalb des Quellsystems verwahren

## SFTP konfigurieren

```bash
rclone config
```

Remote vom Typ `sftp` anlegen und anschließend testen:

```bash
rclone lsd hostbrr:
rclone about hostbrr:
```

`rclone about` funktioniert nur, wenn das Backend die Quota-/Usage-Abfrage unterstützt; deshalb ist dies auf HostBrr praktisch zu verifizieren.

Keinen Port aus Foren blind übernehmen.

## Verschlüsseltes Remote

Ein zweites Remote vom Typ `crypt` über dem SFTP-Ziel anlegen, beispielsweise auf:

```text
hostbrr:/backups/encrypted
```

Die SFTP-Verbindung verschlüsselt nur den Transport. `crypt` verschlüsselt die Nutzdaten und – je nach Konfiguration – Datei- und Verzeichnisnamen clientseitig.

## `copy` oder `sync`?

Für eine Offsite-Kopie ist `copy` häufig die sicherere Ausgangsbasis:

```bash
rclone copy /srv/data hostbrr-crypt:server01 \
  --progress \
  --transfers 4 \
  --checkers 8
```

`sync` macht das Ziel passend zur Quelle und kann deshalb auch Löschungen übertragen. Vor jedem neuen `sync`-Konzept zuerst mit Testdaten und `--dry-run` arbeiten.

## Integritätsprüfung

Für unverschlüsselte Ziele:

```bash
rclone check /srv/data hostbrr:server01
```

Bei einem `crypt`-Remote ist `rclone cryptcheck` das spezialisierte Werkzeug:

```bash
rclone cryptcheck /srv/data hostbrr-crypt:server01
```

Wichtig für SFTP: Remote-Hashes sind nicht immer verfügbar. rclone versucht je nach Serverkonfiguration Hash-Kommandos über SSH zu nutzen. Wenn keine brauchbaren Hashes verfügbar sind, kann `rclone check --download` Daten tatsächlich lesen und vergleichen – das verursacht entsprechend viel Traffic.

## Versionierung und Retention

`copy` + `crypt` allein erzeugt keine komfortable Snapshot-Historie. Für echte Backupgenerationen gibt es drei saubere Ansätze:

1. bereits versionierte Archive übertragen, z. B. Proxmox `vzdump`;
2. getrennte datierte Zielverzeichnisse plus eigene Retention;
3. statt rclone ein Snapshot-Backupwerkzeug wie Restic/Kopia/Borg verwenden.

Für einfache Spiegelungen ist rclone hervorragend; für komplexe Backuphistorien sollte die zusätzliche Logik bewusst geplant werden.

## Restore

Ein Restore muss mit leerem Ziel getestet werden:

```bash
mkdir -p /restore-test
rclone copy hostbrr-crypt:server01 /restore-test --progress
```

Danach Dateizahl, Größen und bei wichtigen Daten Inhalte/Checksums prüfen.

Für Disaster Recovery zusätzlich die rclone-Konfiguration beziehungsweise alle Crypt-Passwörter separat sichern. Ohne Crypt-Geheimnisse sind die Remote-Daten nicht sinnvoll wiederherstellbar.

## Automatisierung

Für Cron/systemd:

- absolute Pfade verwenden,
- Logs schreiben,
- Exitcode prüfen,
- Secrets nicht in die Kommandozeile schreiben,
- keine parallelen Läufe desselben Jobs zulassen,
- Quota/Transferverbrauch beobachten.

## Performance

`--transfers` und `--checkers` nicht blind maximieren. Shared-Hosting-Limits, SFTP-Verschlüsselung, kleine Dateien, Latenz und Quellstorage können begrenzen.

Unsere spätere 2-TB-/8-TB-Testreihe soll identische Daten mit mehreren Parallelitätsstufen übertragen.

## Typische Fehler

### Anmeldung funktioniert in SSH, aber nicht in rclone

Port, Benutzer, Key-Datei und SSH-Agent prüfen. Viele Keys im Agent können zu `Too many authentication failures` führen; rclone unterstützt eine gezieltere Agent-Nutzung.

### `check` kann keine Hashes vergleichen

SFTP-Backend und verfügbare Remote-Hash-Kommandos prüfen. Für einen vollständigen Vergleich notfalls `--download` verwenden.

### Ziel wurde nach `sync` gelöscht

Das ist erwartetes Sync-Verhalten. Für Backups `copy`, Versionierung oder ein Snapshot-Tool verwenden.

### Crypt-Remote lässt sich nach Neuinstallation nicht öffnen

rclone-Konfiguration bzw. Crypt-Passwort/Salt fehlen. Diese Daten gehören in die Disaster-Recovery-Dokumentation.

## HostBrr-spezifische offene Tests

- SSH-Key-Anmeldung auf beiden Boxen
- `rclone about`/Quota-Unterstützung
- verfügbare Hash-Kommandos auf dem SFTP-Server
- `copy`, `check`, `cryptcheck`, Restore
- 10-GiB-Datei und viele kleine Dateien
- optimale `transfers`/`checkers`
- Verhalten bei Verbindungsabbruch

## Primärquellen

- rclone SFTP: https://rclone.org/sftp/
- rclone crypt: https://rclone.org/crypt/
- rclone check: https://rclone.org/commands/rclone_check/
- rclone cryptcheck: https://rclone.org/commands/rclone_cryptcheck/
