# Bestandsaufnahme & Roadmap

Stand: 24. August 2026

## Reifegrad

| Bereich | Reifegrad | Wichtigste offene Punkte |
|---|---|---|
| Grundlagen | hoch | aktuelle Limits/Änderungen beobachten |
| DirectAdmin/Zugang | hoch | HostBrr-Menüatlas, Screenshots, reale Funktionen |
| Backup | hoch | Borg aktuell verifizieren, Restore-Zeiten |
| Mounts | mittel | Langzeitstabilität und Cache-Tuning |
| Anwendungen | mittel | Nextcloud-Praxis und App-Grenzen |
| Sicherheit | hoch konzeptionell | Löschschutz und Credential-Trennung praktisch prüfen |
| Performance | mittel | eigene reproduzierbare Messungen fehlen |
| Troubleshooting | mittel-hoch | echte Fehlerfälle laufend ergänzen |
| Rezepte | hoch/wachsend | Praxisprüfung und weitere Recovery-Fälle |
| Quellen | mittel | Reddit, WebHostingTalk, NodeSeek weiter ausbauen |
| Eigene Tests | zurückgestellt | 2-TB-/8-TB-Vergleich später |

## Bereits besonders stark

**DirectAdmin:** Ersteinrichtung, Pfade, SSH-Keys, Cron, Domains/SSL, Datenbanken, PHP/LiteSpeed, Softaculous und API.

**Backup:** rsync, rclone crypt, Restic, Borg und Proxmox plus Entscheidungshilfe.

**Sicherheit:** Bedrohungsmodell, Verschlüsselung, SSH-Key-Härtung, 3-2-1 sowie Ransomware-/Löschschutz.

**Rezepte:** konkrete Aufgaben statt nur Werkzeugdokumentation; dieser Bereich soll langfristig der wichtigste Nutzereinstieg werden.

## Priorität A – vor Version 1.0

### Aktuelle HostBrr-Ressourcenlimits
Zu klären: CPU, RAM, I/O, IOPS, Prozesse/Tasks, Cron-/PHP-Limits und sinnvolle Transferparallelität. Historische Community-Werte bleiben bis zur Verifikation als solche gekennzeichnet.

### Reale Shell-Umgebung
Später auf beiden Testboxen erfassen:

```bash
uname -a
id
pwd
php -v
rsync --version
rclone version
restic version
borg --version
ssh -V
```

### DirectAdmin-Menüatlas
Echte Screenshots von Dashboard, File Manager, SSH Keys, Cron Jobs, Domains, SSL, Datenbanken, PHP, Softaculous und Resource Usage/Quota.

### Restore-Praxis
Für jedes wichtige Backupverfahren mindestens einen vollständigen Restore dokumentieren.

## Priorität B

**Nextcloud:** Softaculous, Cron, PHP-Extensions/Limits, OPcache, Datenbankperformance, große Uploads, WebDAV, SFTP External Storage und Upgrades praktisch prüfen.

**NAS:** Synology/QNAP gegen eine reale Box prüfen, besonders rsync, SSH-Port, Verschlüsselung und Restore.

**Mounts:** Langzeittests, Reconnect, Cache-Größen und viele kleine Dateien.

## Priorität C

Weitere Quellen: Reddit, WebHostingTalk, NodeSeek, ältere LowEndTalk-Angebotsthreads sowie HostBrr-KB/Status-/Produktseiten. Datum und Produktgeneration müssen erhalten bleiben.

Weitere Rezepte: Linux-Desktop, macOS, Datenbankserver, WordPress, Medienarchive, Hetzner→HostBrr-Migration, HostBrr→anderes Ziel, 2-TB→8-TB-Migration und Backup-Integritätsreports.

## Spätere Praxisphase: 2 TB vs. 8 TB

Verglichen werden sollen Node/Standort, DirectAdmin-Version, Ressourcenlimits, Shell-Tools, große und kleine Dateien, SFTP, rsync, rclone, Restic/Borg soweit verfügbar, Parallelität, Metadatenoperationen und Langzeitstabilität.

## Kriterien für Version 1.0

Version 1.0 ist erreicht, wenn die wichtigsten Einstiegspfade vollständig sind, zentrale Howtos einen Restore-Weg enthalten, aktuelle und historische Produktdaten getrennt sind, zentrale Aussagen Quellen besitzen, mindestens eine reale StorageBox inventarisiert wurde und offene Behauptungen sichtbar als unbestätigt markiert sind.
