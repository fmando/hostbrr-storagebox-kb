---
title: DirectAdmin auf der HostBrr StorageBox
category: zugang
status: official-plus-hostbrr-review
last_reviewed: 2026-08-24
---

# DirectAdmin auf der HostBrr StorageBox

DirectAdmin ist bei der HostBrr StorageBox nicht nur ein Webhosting-Panel, sondern die zentrale Verwaltungsoberfläche für viele Funktionen der Box. Da die StorageBox **kein Root-VPS** ist, werden zahlreiche Aufgaben, die man auf einem eigenen Linux-Server als root erledigen würde, stattdessen über DirectAdmin oder innerhalb des eigenen Benutzerkontos ausgeführt.

> Wichtig: Welche Menüpunkte tatsächlich sichtbar sind, hängt vom von HostBrr freigeschalteten Paket und der eingesetzten DirectAdmin-Version ab. Die allgemeine DirectAdmin-Dokumentation beschreibt auch Funktionen für Server-Administratoren, die ein normaler StorageBox-Benutzer nicht besitzt.

## 1. DirectAdmin öffnen

DirectAdmin lauscht standardmäßig auf TCP-Port `2222`. Die konkrete URL, der Hostname und die Zugangsdaten der StorageBox sollten jedoch immer aus der HostBrr-Welcome-Mail bzw. dem Kundenbereich übernommen werden.

Allgemeines Schema:

```text
https://HOSTNAME:2222/
```

Offizielle Dokumentation:

- [DirectAdmin – General Usage / Accessing DirectAdmin Panel](https://docs.directadmin.com/directadmin/general-usage/)

## 2. Evolution-Oberfläche

Bei aktuellen DirectAdmin-Installationen ist **Evolution** die moderne Standardoberfläche. DirectAdmin beschreibt daneben noch den älteren `Enhanced`-Skin. Menübezeichnungen können sich deshalb zwischen Screenshots, älteren Forenbeiträgen und der aktuellen HostBrr-Installation unterscheiden.

Offizielle Dokumentation:

- [DirectAdmin – Evolution Skin](https://docs.directadmin.com/directadmin/skins-and-templates/evolution.html)
- [DirectAdmin – Skins and Templates](https://docs.directadmin.com/directadmin/skins-and-templates/)

## 3. Welche Rolle hat DirectAdmin bei der StorageBox?

Für unsere KB betrachten wir DirectAdmin in acht Funktionsgruppen:

| Bereich | Typische Aufgabe |
|---|---|
| Account | Passwort, 2FA, Login-Sicherheit |
| Dateien | File Manager, Verzeichnisse, Rechte |
| SSH | SSH-Keys und Zugang vorbereiten |
| Cron | automatische Backup-/Synchronisationsjobs |
| Domains | Domains und Subdomains verwalten |
| SSL | TLS-Zertifikate / ACME / Let's Encrypt |
| Datenbanken | Datenbanken für Webanwendungen |
| Anwendungen | Softaculous bzw. bereitgestellte Webapps |

Welche dieser Funktionen HostBrr auf einer konkreten StorageBox freischaltet, muss paket- bzw. generationsbezogen dokumentiert werden.

## 4. Account absichern

Nach der Ersteinrichtung sollte die Absicherung des DirectAdmin-Kontos zu den ersten Schritten gehören.

DirectAdmin unterstützt unter anderem **Two-Step Authentication (2FA)**. Laut offizieller Dokumentation befindet sich die Funktion im Benutzerbereich unter `Change your Password` → `Two-Step Authentication`; die genaue Darstellung kann im Evolution-Skin variieren.

Offizielle Dokumentation:

- [DirectAdmin – Securing DirectAdmin](https://docs.directadmin.com/directadmin/general-usage/securing-da-panel.html)

Empfehlung für die StorageBox:

1. eigenes starkes DirectAdmin-Passwort verwenden,
2. 2FA aktivieren, sofern von HostBrr freigeschaltet,
3. für automatisierte SSH-Aufgaben SSH-Keys statt Account-Passwort verwenden,
4. keine Zugangsdaten in Skripten oder im Git-Repository speichern.

## 5. SSH und SSH-Keys

SSH ist für die StorageBox besonders wichtig, weil darauf unter anderem SFTP, rsync und verschiedene Backupverfahren aufbauen.

Die eigentliche SSH-Konfiguration des Servers kontrolliert HostBrr. Der Benutzer verwaltet nur die ihm zur Verfügung gestellten Funktionen und seine Schlüssel.

Unser ausführliches HostBrr-Howto:

- [SSH & SFTP](ssh-sftp.md)

Allgemeine DirectAdmin-Dokumentation:

- [DirectAdmin – Managing with SSH](https://docs.directadmin.com/operation-system-level/os-general/managing-with-ssh.html)

> Hostname, Benutzername und insbesondere ein eventuell abweichender SSH-Port sollten **nicht aus alten Forenbeiträgen übernommen** werden. Maßgeblich sind die Zugangsdaten der eigenen StorageBox.

## 6. File Manager und Verzeichnisstruktur

Der File Manager eignet sich für interaktive Arbeiten wie:

- Verzeichnisse anlegen,
- kleine Dateien hoch- oder herunterladen,
- Dateien umbenennen,
- Dateirechte prüfen,
- Webdateien verwalten.

Für große Datenmengen und regelmäßige Backups sind dagegen SFTP, rsync, rclone, Borg oder Restic meist geeigneter.

Bei DirectAdmin liegen Benutzerinhalte typischerweise unter `/home/USERNAME/`. Die exakte für HostBrr sichtbare Verzeichnisstruktur dokumentieren wir erst als `verified`, wenn sie auf einer aktuellen StorageBox geprüft wurde.

Hintergrund zur DirectAdmin-Verzeichnisstruktur:

- [DirectAdmin – Directories and Locations](https://docs.directadmin.com/directadmin/general-usage/directories-and-locations.html)

## 7. Cronjobs – besonders wichtig für Backups

Cronjobs sind für unsere StorageBox-KB zentral. Damit können wiederkehrende Aufgaben ohne laufenden eigenen VPS-Prozess gestartet werden, beispielsweise:

```text
VPS
  ↑
  │ rsync über SSH
  │
HostBrr StorageBox
  └─ DirectAdmin Cronjob startet den Pull
```

Community-Berichte zeigen genau dieses Muster für regelmäßige rsync-Backups. Vor produktivem Einsatz sollte jeder Befehl zunächst manuell über SSH getestet werden.

Bei Cronjobs beachten:

- immer absolute Pfade verwenden,
- Ausgabe/Fehler protokollieren,
- keine Passwörter in die Kommandozeile schreiben,
- SSH-Key-Authentifizierung verwenden,
- Überschneidungen lang laufender Jobs vermeiden,
- Speicher- und Ressourcenlimits der Shared-Hosting-Umgebung berücksichtigen.

DirectAdmin speichert Benutzer-Cronjobs intern in einer eigenen Konfiguration; ob und welche Cron-Funktion verfügbar ist, kann vom Benutzerpaket abhängen.

## 8. Domains und Subdomains

Die StorageBox kann mehr als ein reines SFTP-Ziel sein. HostBrr stellt Webhosting-Funktionen bereit, weshalb Domains/Subdomains beispielsweise für folgende Zwecke interessant sind:

- Nextcloud,
- WebDAV/Webanwendungen,
- statische Downloadbereiche,
- eigene Status- oder Landingpages.

Eine Domain sollte nur dann auf die StorageBox zeigen, wenn der entsprechende Dienst tatsächlich öffentlich erreichbar sein soll.

## 9. SSL / Let's Encrypt / ACME

DirectAdmin besitzt integrierte ACME-Unterstützung und kann unter anderem kostenlose Zertifikate von Let's Encrypt bzw. anderen unterstützten ACME-Anbietern ausstellen und erneuern. Im User Level befindet sich die Zertifikatsverwaltung unter `SSL Certificates`, sofern die Funktion vom Provider aktiviert wurde.

Offizielle Dokumentation:

- [DirectAdmin – SSL and Let's Encrypt for Domains](https://docs.directadmin.com/webservices/ssl/ssl-and-letsencrypt-for-domains.html)

Das ist insbesondere relevant, wenn auf der StorageBox Nextcloud oder eine andere Webanwendung unter einer eigenen Domain betrieben wird.

## 10. Datenbanken

HostBrr bewirbt bei der StorageBox auch Webhosting-/Datenbankfunktionen. DirectAdmin dient dabei als Verwaltungsoberfläche für die vom Paket bereitgestellten Datenbanken.

Für unsere KB müssen wir noch praktisch erfassen:

- welches DB-System HostBrr aktuell bereitstellt,
- welche Version,
- Datenbank-/Benutzerlimits,
- Speicherlimits,
- Zugriff nur lokal oder auch remote,
- Backup- und Restoremöglichkeiten.

Gerade für Nextcloud ist die Datenbank relevant. Die StorageBox sollte jedoch nicht automatisch wie ein eigener Datenbankserver behandelt werden: CPU-, RAM- und I/O-Ressourcen unterliegen Shared-Hosting-/LVE-Limits.

## 11. Softaculous und Anwendungen

HostBrr nennt Softaculous und die Möglichkeit, Webanwendungen wie Nextcloud zu installieren. Softaculous ist dabei ein zusätzlicher Installer innerhalb bzw. neben der Hosting-Verwaltung und **nicht Teil von DirectAdmin selbst**.

Offizielle Links:

- [Softaculous](https://www.softaculous.com/)
- [Nextcloud](https://nextcloud.com/)
- [Nextcloud Admin Manual](https://docs.nextcloud.com/server/latest/admin_manual/)

Unser HostBrr-spezifischer Artikel:

- [Nextcloud auf der StorageBox](../05-anwendungen/nextcloud.md)

## 12. DirectAdmin API – später interessant für Automatisierung

DirectAdmin besitzt eine API. Aktuelle Versionen stellen eine neue JSON-API unter `/api/...` bereit; daneben existiert die ältere `CMD_API_...`-Schnittstelle. Im Evolution-Skin gibt es außerdem eine **Live API Documentation**.

Offizielle Dokumentation:

- [DirectAdmin – API Access](https://docs.directadmin.com/developer/api/)

Für die KB ist das später interessant, um beispielsweise Informationen automatisiert auszulesen. Ob HostBrr alle dafür benötigten API-Funktionen für StorageBox-Benutzer freigibt, muss vor Nutzung geprüft werden.

## 13. Was DirectAdmin nicht bedeutet

Der wichtigste Punkt für das Verständnis der StorageBox:

**DirectAdmin-Zugang ist kein Root-Zugang.**

Ein normaler StorageBox-Benutzer kann daher nicht davon ausgehen, dass er:

- Systempakete installieren,
- Systemdienste konfigurieren,
- Firewallregeln ändern,
- DirectAdmin selbst administrieren,
- den SSH-Daemon konfigurieren,
- beliebige Hintergrunddienste dauerhaft betreiben

kann.

Viele Seiten der offiziellen DirectAdmin-Dokumentation richten sich an Administratoren eigener DirectAdmin-Server. Diese Befehle sind auf einer HostBrr StorageBox nicht automatisch ausführbar.

## 14. Geplanter HostBrr-Screenshot-/Menüatlas

Sobald wir Zugriff auf eine aktuelle StorageBox haben, soll dieser Artikel um einen **StorageBox-spezifischen Menüatlas** ergänzt werden:

```text
DirectAdmin
├── Account Manager
├── Advanced Features
├── System Info & Files
├── Extra Features
└── ggf. Softaculous
```

Für jeden tatsächlich sichtbaren Menüpunkt dokumentieren wir dann:

- exakten Namen,
- Screenshot,
- Zweck,
- HostBrr-spezifische Einschränkungen,
- getestete Funktionen,
- Link zur offiziellen DirectAdmin-Dokumentation.

Dadurch vermeiden wir, dass die KB lediglich eine allgemeine DirectAdmin-Anleitung wird.

## 15. Checkliste nach Bestellung einer StorageBox

Nach Bereitstellung einer neuen StorageBox sollten wir zunächst erfassen:

- [ ] DirectAdmin-URL und Version
- [ ] Evolution-/Enhanced-Skin
- [ ] sichtbare Menüpunkte
- [ ] Speicherquota und Nutzung
- [ ] SSH aktiviert?
- [ ] SSH-Port
- [ ] SSH-Key-Verwaltung vorhanden?
- [ ] Cronjobs vorhanden?
- [ ] Domains/Subdomains möglich?
- [ ] SSL/ACME verfügbar?
- [ ] Datenbanktyp und Version
- [ ] Softaculous vorhanden?
- [ ] PHP-Version(en)
- [ ] File-Manager-Funktionen
- [ ] API/Live API Documentation sichtbar?

Diese Daten werden später als `verified` mit StorageBox-Generation und Standort dokumentiert.

## Weiterführende Dokumentation

- [DirectAdmin Documentation](https://docs.directadmin.com/)
- [DirectAdmin – General Usage](https://docs.directadmin.com/directadmin/general-usage/)
- [DirectAdmin – Evolution Skin](https://docs.directadmin.com/directadmin/skins-and-templates/evolution.html)
- [DirectAdmin – Securing DirectAdmin](https://docs.directadmin.com/directadmin/general-usage/securing-da-panel.html)
- [DirectAdmin – Directories and Locations](https://docs.directadmin.com/directadmin/general-usage/directories-and-locations.html)
- [DirectAdmin – SSL / Let's Encrypt](https://docs.directadmin.com/webservices/ssl/ssl-and-letsencrypt-for-domains.html)
- [DirectAdmin – API Access](https://docs.directadmin.com/developer/api/)
