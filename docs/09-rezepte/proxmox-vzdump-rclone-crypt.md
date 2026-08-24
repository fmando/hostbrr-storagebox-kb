---
title: Proxmox vzdump verschlüsselt offsite sichern
category: rezepte
status: maintained
last_reviewed: 2026-08-24
---
# Proxmox vzdump verschlüsselt auf HostBrr sichern

## Ziel

VM- und LXC-Backups werden zunächst mit Proxmox lokal erzeugt und anschließend verschlüsselt auf die HostBrr StorageBox übertragen.

## Empfohlene Architektur

```text
Proxmox VE
   |
   | vzdump
   v
lokales Backup-Verzeichnis
   |
   | rclone crypt über SFTP
   v
HostBrr StorageBox
```

Damit bleibt die eigentliche Proxmox-Backup-Erstellung unabhängig von der WAN-Verbindung.

Offizielle Dokumentation:

- [Proxmox VE – Backup and Restore](https://pve.proxmox.com/pve-docs/chapter-vzdump.html)
- [rclone – SFTP](https://rclone.org/sftp/)
- [rclone – crypt](https://rclone.org/crypt/)
- [rclone – copy](https://rclone.org/commands/rclone_copy/)

## Warum nicht einfach PBS über SSHFS/rclone mount?

Ein Remote-FUSE-/SFTP-Mount fügt einen zusätzlichen Dateisystem-Layer zwischen PBS und seinen Datastore ein. Für native PBS-Datastores sollte deshalb eine von Proxmox unterstützte Storage-Architektur verwendet werden.

Für klassische `vzdump`-Archive ist das Modell wesentlich einfacher: Proxmox erstellt fertige Backup-Dateien, rclone überträgt sie anschließend offsite.

## Schritt 1 – lokales Backup

Beispiel:

```bash
vzdump 101 --dumpdir /var/lib/vz/dump --mode snapshot --compress zstd
```

In produktiven Umgebungen können die Backups selbstverständlich über die Proxmox-GUI geplant werden.

## Schritt 2 – SFTP-Remote

```bash
rclone config
```

Remote beispielsweise `hostbrr` nennen und als Backend `sftp` wählen.

Host, Benutzer, Port und SSH-Key entsprechend der StorageBox konfigurieren.

Test:

```bash
rclone lsd hostbrr:
```

## Schritt 3 – crypt-Layer

Erneut:

```bash
rclone config
```

Neues Remote vom Typ `crypt`, beispielsweise `hostbrr-crypt`.

Als Ziel etwa:

```text
hostbrr:proxmox-encrypted
```

Passwörter außerhalb dieser Wissensdatenbank sicher dokumentieren.

## Schritt 4 – übertragen

Für eine Offsite-Kopie ist `copy` häufig sicherer verständlich als `sync`, weil entfernte lokale Dateien nicht automatisch auf dem Ziel gelöscht werden:

```bash
rclone copy /var/lib/vz/dump hostbrr-crypt:node01 \
  --progress \
  --transfers 4
```

Erst wenn eine gewünschte Lösch-/Retentionstrategie feststeht, sollte `sync` eingesetzt werden.

## Restore

Zuerst Archiv zurückholen:

```bash
rclone copy hostbrr-crypt:node01 /var/lib/vz/dump --progress
```

Danach Restore mit den normalen Proxmox-Werkzeugen.

## Automatisierung

Die saubere Reihenfolge lautet:

1. vzdump erfolgreich abschließen
2. Exit-Code prüfen
3. Offsite-Transfer starten
4. Transfer prüfen
5. lokale Retention ausführen
6. Remote-Retention bewusst getrennt behandeln

## Sicherheit

Die StorageBox sollte nicht die einzige Kopie sein. Besonders wertvoll ist eine lokale Backupgeneration plus verschlüsselte Offsite-Kopie.

## Später testen

Auf 2-TB- und 8-TB-Box vergleichen wir unter anderem:

- Transfer mit 1/2/4/8 parallelen Streams
- große VMA/ZST-Dateien
- Resume nach Verbindungsabbruch
- Restore-Geschwindigkeit
- tatsächliche Auswirkung des Transferkontingents
