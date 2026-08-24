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
- [rclone – cryptcheck](https://rclone.org/commands/rclone_cryptcheck/)

## Warum nicht einfach PBS über SSHFS/rclone mount?

Ein Remote-FUSE-/SFTP-Mount fügt einen zusätzlichen Dateisystem-Layer zwischen PBS und seinen Datastore ein. Für native PBS-Datastores sollte deshalb eine von Proxmox unterstützte Storage-Architektur verwendet werden.

Für klassische `vzdump`-Archive ist das Modell wesentlich einfacher: Proxmox erstellt fertige Backup-Dateien, rclone überträgt sie anschließend offsite.

## Schritt 1 – lokales Backup

Beispiel:

```bash
vzdump 101 --dumpdir /var/lib/vz/dump --mode snapshot --compress zstd
```

In produktiven Umgebungen können die Backups über die Proxmox-GUI geplant werden.

## Schritt 2 – SFTP-Remote

```bash
rclone config
```

Remote beispielsweise `hostbrr` nennen und als Backend `sftp` wählen. Host, Benutzer, Port und SSH-Key entsprechend der StorageBox konfigurieren.

Test:

```bash
rclone lsd hostbrr:
```

Bei SFTP sind serverseitige Hashes nicht grundsätzlich garantiert. rclone kann sie bei vorhandenem Shell-Zugriff über Remote-Kommandos ermitteln; ohne diese Möglichkeit muss die Verifikationsstrategie entsprechend angepasst werden.

## Schritt 3 – crypt-Layer

```bash
rclone config
```

Neues Remote vom Typ `crypt`, beispielsweise `hostbrr-crypt`.

Als Ziel etwa:

```text
hostbrr:proxmox-encrypted
```

Passwörter außerhalb dieser Wissensdatenbank sicher dokumentieren. Standardmäßig kann rclone crypt auch Datei- und Verzeichnisnamen verschlüsseln.

## Schritt 4 – übertragen

Für eine Offsite-Kopie ist `copy` häufig sicherer verständlich als `sync`, weil entfernte lokale Dateien nicht automatisch auf dem Ziel gelöscht werden:

```bash
rclone copy /var/lib/vz/dump hostbrr-crypt:node01 \
  --progress \
  --transfers 4
```

Erst wenn eine gewünschte Lösch-/Retentionstrategie feststeht, sollte `sync` eingesetzt werden.

## Schritt 5 – Transfer verifizieren

Bei einem `crypt`-Remote sollte zur Integritätsprüfung bevorzugt `cryptcheck` verwendet werden:

```bash
rclone cryptcheck /var/lib/vz/dump hostbrr-crypt:node01
```

Für SFTP-Backends ohne nutzbare Remote-Hashes kann eine vollständige Prüfung zusätzlichen Download-Traffic verursachen. Das sollte insbesondere bei Multi-TB-Beständen bewusst geplant werden.

## Restore

Zuerst Archiv zurückholen:

```bash
rclone copy hostbrr-crypt:node01 /var/lib/vz/dump --progress
```

Danach Restore mit den normalen Proxmox-Werkzeugen. Für einen echten Test sollte eine einzelne VM oder ein LXC in einer isolierten Umgebung vollständig zurückgespielt und gestartet werden.

## Automatisierung

Die saubere Reihenfolge lautet:

1. `vzdump` erfolgreich abschließen,
2. Exit-Code prüfen,
3. Offsite-Transfer starten,
4. Transfer verifizieren,
5. lokale Retention ausführen,
6. Remote-Retention bewusst getrennt behandeln,
7. Fehler protokollieren bzw. melden.

Lokale Backups sollten nicht gelöscht werden, bevor der Offsite-Transfer erfolgreich abgeschlossen und geprüft wurde.

## Sicherheit

Die StorageBox sollte nicht die einzige Kopie sein. Besonders wertvoll ist eine lokale Backupgeneration plus verschlüsselte Offsite-Kopie.

Die rclone-Konfiguration enthält sensible Informationen für den `crypt`-Layer und muss selbst gesichert werden. Ohne die Crypt-Zugangsdaten ist der Offsite-Bestand nicht sinnvoll wiederherstellbar.

Weiterlesen:

- [rclone + SFTP + crypt](../03-backup/rclone-sftp-crypt.md)
- [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md)
- [Ransomware & Löschschutz](../06-sicherheit/ransomware-loeschschutz.md)
- [Disaster Recovery](disaster-recovery.md)

## Typische Fehler

### `rclone copy` funktioniert, `cryptcheck` aber nicht wie erwartet

Prüfen, ob wirklich gegen das `crypt`-Remote und nicht direkt gegen das darunterliegende SFTP-Remote geprüft wird. Außerdem beachten, dass SFTP selbst keine nativen Datei-Hashes bereitstellt.

### Transfer ist bei großen vzdump-Dateien langsam

Zunächst Netzwerkpfad, Quellstorage und CPU prüfen. Die Zahl paralleler Transfers nicht blind erhöhen; bei wenigen sehr großen Archiven kann eine geringe Parallelität bereits sinnvoll sein.

### Remote-Dateien werden versehentlich gelöscht

`copy` und `sync` unterscheiden sich grundlegend. `sync` kann Dateien am Ziel entfernen, damit es der Quelle entspricht. Für reine Offsite-Archivierung deshalb zunächst `copy` verwenden.

### Restore ist zu langsam

Das ist Teil des Recovery Designs. RTO und Datenmenge müssen zusammen betrachtet werden; ein Multi-TB-Backup kann korrekt sein und trotzdem zu langsam für das gewünschte Wiederanlaufziel.

## Später testen

Auf 2-TB- und 8-TB-Box vergleichen wir unter anderem:

- Transfer mit 1/2/4/8 parallelen Streams
- große VMA/ZST-Dateien
- Resume nach Verbindungsabbruch
- `cryptcheck` und Hash-Verhalten über SFTP
- Restore-Geschwindigkeit
- tatsächliche Auswirkung des Transferkontingents
