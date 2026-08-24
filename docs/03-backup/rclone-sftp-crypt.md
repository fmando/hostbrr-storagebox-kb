---
title: rclone über SFTP mit optionaler Verschlüsselung
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# rclone über SFTP mit optionaler Verschlüsselung

`rclone` über SFTP ist eine der am häufigsten berichteten Kombinationen für die HostBrr StorageBox. Mehrere Community-Berichte nennen sowohl normale Transfers als auch Mounts und `rclone crypt`.

> **Status:** Community mehrfach bestätigt, in dieser KB noch nicht selbst verifiziert.

## Architektur

```text
Quellsystem
   |
   | rclone
   v
optional: crypt remote
   |
   | SFTP/SSH
   v
HostBrr StorageBox
```

## Warum rclone crypt?

Die SFTP-Verbindung verschlüsselt den Transport. Ohne zusätzliche Verschlüsselung liegen die Dateien auf dem Ziel jedoch grundsätzlich im Klartext vor. Ein `crypt`-Remote verschlüsselt Inhalte und optional Dateinamen bereits auf dem Client.

Auch bei aktivierter Verschlüsselung können bestimmte Metadaten ableitbar bleiben, insbesondere Dateigrößen und ungefähr die Länge verschlüsselter Pfade/Dateinamen.

## Basiskonfiguration

Auf dem Client:

```bash
rclone config
```

Ein neues Remote vom Typ `sftp` anlegen. Benötigt werden die individuellen Daten aus der HostBrr-Welcome-Mail bzw. DirectAdmin-Konfiguration:

- Hostname/IP
- Benutzername
- SSH-Port
- bevorzugt SSH-Key statt Passwort

**Keinen SSH-Port aus Foren blind übernehmen.** In Community-Beispielen taucht Port `53211` auf; maßgeblich sind die Daten des eigenen Accounts.

Anschließend testen:

```bash
rclone lsd hostbrr:
```

## Verschlüsseltes Remote

Danach ein zweites Remote vom Typ `crypt` erstellen:

```bash
rclone config
```

Als darunterliegendes Ziel beispielsweise:

```text
hostbrr:/backups/encrypted
```

Passwort und Salt/zweites Passwort sicher außerhalb des Repositories aufbewahren. Ohne diese Daten ist ein Restore nicht möglich.

## Backup-Beispiel

Für Backup-Szenarien ist `copy` häufig sicherer als ein unbedachtes `sync`, weil `sync` Löschungen auf das Ziel spiegeln kann.

```bash
rclone copy /srv/data hostbrr-crypt:server01 \
  --progress \
  --transfers 4 \
  --checkers 8
```

Vor produktiver Nutzung zuerst mit Testdaten arbeiten.

## Mounts und VFS Cache

Community-Berichte empfehlen bei `rclone mount` einen lokalen VFS-Cache. Das kann Schreibzugriffe und wiederholte Zugriffe deutlich beschleunigen. Die optimale Cachegröße hängt vom Client und Anwendungsfall ab.

Ein Community-Nutzer wechselte bei einem datenintensiven Anwendungsfall mit vielen Metadatenzugriffen von rclone mount zu JuiceFS über SFTP, weil Metadatenoperationen über rclone/SFTP langsam waren. Für reine Backup-Transfers ist dieses Problem wesentlich weniger relevant.

## Noch zu testen

- SSH-Key-Anmeldung auf aktueller 2026er StorageBox
- tatsächlicher SSH-Port
- `rclone lsd`, `copy`, `check`, `sync`
- `crypt` mit verschlüsselten Datei- und Verzeichnisnamen
- Upload/Download einer 10-GiB-Testdatei
- 10.000 kleine Dateien
- Restore auf leeres Ziel
- sinnvolle Werte für `transfers` und `checkers`

## Quellen

- https://lowendtalk.com/discussion/206272/hostbrr-storage-box-how-safe-is-it
- https://lowendtalk.com/discussion/205325/hostbrr-storage-boxes-any-experiences-with-them-good-or-bad
- https://lowendtalk.com/discussion/202975/tutorial-turn-a-cheap-hdd-storage-vps-into-nvme-speed
- https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p15
