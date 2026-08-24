---
title: "DirectAdmin: Ersteinrichtung der HostBrr StorageBox"
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---

# DirectAdmin: Ersteinrichtung

Dieses Howto beschreibt die ersten Schritte nach Bereitstellung einer HostBrr StorageBox. Da HostBrr DirectAdmin als Shared-Hosting-Control-Panel bereitstellt, können sichtbare Funktionen je nach Tarif, Server und freigeschalteten Features abweichen.

## 1. Zugangsdaten sichern

Die HostBrr-Welcome-Mail sollte mindestens Hostname, DirectAdmin-Zugang und gegebenenfalls abweichende SSH/SFTP-Daten enthalten. Zugangsdaten gehören nicht in Git-Repositories oder unverschlüsselte Notizen.

## 2. In DirectAdmin anmelden

Die exakte URL aus der HostBrr-Welcome-Mail verwenden. DirectAdmin verwendet standardmäßig Port 2222, HostBrr-spezifische Angaben haben jedoch Vorrang.

Offizielle Dokumentation: [DirectAdmin Knowledge Base](https://docs.directadmin.com/)

## 3. Oberfläche inventarisieren

Nach der ersten Anmeldung notieren wir die tatsächlich sichtbaren Bereiche. Besonders relevant sind:

- Account Manager
- System Info & Files
- Advanced Features
- SSH Keys
- Cron Jobs
- File Manager
- Domain Setup / Domain Management
- SSL Certificates
- MySQL Management
- Softaculous

Nicht jede generische DirectAdmin-Funktion muss bei HostBrr verfügbar sein.

## 4. Passwort und Kontosicherheit

Das initiale Passwort sollte durch ein einzigartiges starkes Passwort ersetzt werden. Falls Two-Step Authentication im Benutzerkonto angeboten wird, sollte sie aktiviert werden.

Offizielle Dokumentation: [Securing DirectAdmin](https://docs.directadmin.com/directadmin/general-usage/securing-da-panel.html)

## 5. Speicher und Quota prüfen

Vor dem ersten großen Transfer prüfen:

- gebuchter Speicher
- belegter Speicher
- Bandbreitenanzeige
- Inode-Anzeige, falls vorhanden
- Account-/Systeminformationen

Die Werte dienen später als Referenz für Troubleshooting und Performance-Beobachtungen.

## 6. File Manager öffnen

Der File Manager hilft dabei, die tatsächliche Home-Verzeichnisstruktur der StorageBox zu verstehen. Vor Automatisierung niemals blind Pfade aus fremden DirectAdmin-Anleitungen übernehmen.

## 7. SSH/SFTP prüfen

Falls SSH freigeschaltet ist, anschließend den Zugang testen. Danach sollte ein SSH-Key eingerichtet werden.

Weiter: [SSH & SFTP](ssh-sftp.md) und [SSH-Key über DirectAdmin](directadmin-ssh-key.md).

## 8. Cron prüfen

Wenn Cron Jobs sichtbar sind, können später automatisierte Pull-Backups, Wartungsjobs oder Skripte geplant werden.

Weiter: [Cronjobs in DirectAdmin](directadmin-cronjobs.md).

## 9. Domains und SSL nur bei Bedarf

Wer die StorageBox ausschließlich als SFTP-/Backupziel nutzt, benötigt nicht zwingend eine eigene Domain. Für Nextcloud, WebDAV oder Webhosting sind Domain und TLS dagegen sinnvoll.

Weiter: [Domains und SSL](directadmin-domains-ssl.md).

## Ersteinrichtungs-Checkliste

- [ ] Welcome-Mail archiviert
- [ ] DirectAdmin-Login funktioniert
- [ ] Passwort geändert
- [ ] 2FA geprüft/aktiviert
- [ ] Tarif/Quota kontrolliert
- [ ] sichtbare DirectAdmin-Menüs dokumentiert
- [ ] Home-Verzeichnisstruktur geprüft
- [ ] SSH/SFTP getestet
- [ ] SSH-Key eingerichtet
- [ ] Cron-Verfügbarkeit geprüft
- [ ] Datenbanken/PHP/Softaculous bei Bedarf geprüft
- [ ] Domain/SSL bei Bedarf eingerichtet

## HostBrr-Verifikation

Sobald eine aktuelle StorageBox verfügbar ist, sollten Screenshots und die exakten Menübezeichnungen ergänzt werden. Erst dann wird dieses Howto auf `verified` gesetzt.
