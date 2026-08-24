---
title: Nextcloud direkt auf der StorageBox
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Nextcloud direkt auf der HostBrr StorageBox

## Ziel

Nextcloud läuft vollständig im HostBrr-DirectAdmin-Account: PHP/Webserver, Datenbank und Daten liegen auf der StorageBox. Das ist die einfachste Architektur, aber auch diejenige mit der stärksten Abhängigkeit von Shared-Hosting-Limits.

## Architektur

```text
Browser / Apps
      ↓ HTTPS
HostBrr DirectAdmin / LiteSpeed
      ↓
Nextcloud + Datenbank + Storage
auf der StorageBox
```

## Geeignet für

Eher passend für private bzw. moderate Nutzung, wenn Einfachheit wichtiger ist als vollständige Serverkontrolle.

Weniger passend für stark ausgelastete, datenbank-/metadatenintensive oder geschäftskritische Instanzen.

## Voraussetzungen und Preflight

Vor der Installation dokumentieren:

```text
PHP-Version:
memory_limit:
upload_max_filesize:
post_max_size:
max_execution_time:
Datenbanktyp/-version:
Cron verfügbar:
PHP-CLI-Pfad:
HTTPS/Let's Encrypt:
Quota:
Softaculous Nextcloud-Version:
```

Die Werte müssen zu den Anforderungen der tatsächlich installierten Nextcloud-Version passen.

Offizielle Dokumentation:

- System Requirements: https://docs.nextcloud.com/server/stable/admin_manual/installation/system_requirements.html
- Installation: https://docs.nextcloud.com/server/stable/admin_manual/installation/index.html
- Background Jobs: https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/background_jobs_configuration.html
- DirectAdmin: https://docs.directadmin.com/
- Softaculous: https://www.softaculous.com/

## 1. Domain und TLS

Empfohlen ist eine eigene Subdomain, z. B.:

```text
cloud.example.net
```

DNS konfigurieren, Domain in DirectAdmin anlegen und ein gültiges TLS-Zertifikat aktivieren, bevor produktive Zugangsdaten verwendet werden.

## 2. Installation über Softaculous

Wenn Nextcloud angeboten wird:

1. DirectAdmin öffnen.
2. Softaculous starten.
3. Nextcloud auswählen.
4. Ziel-Domain und Installationspfad festlegen.
5. sichere Admin-/Datenbankdaten setzen.
6. Installation durchführen.
7. Nextcloud-Adminübersicht auf Warnungen prüfen.

Nicht automatisch davon ausgehen, dass die angebotene Version aktuell oder für die vorhandene PHP-Version optimal ist.

## 3. Cron statt AJAX

Für produktive Instanzen empfiehlt Nextcloud **Cron** als Background-Job-Modus. AJAX ist weniger zuverlässig; Webcron ist laut Nextcloud eher für sehr kleine Instanzen geeignet.

Auf HostBrr müssen wir deshalb praktisch bestimmen:

- PHP-CLI-Pfad
- Pfad zu `cron.php`
- zulässige Cron-Frequenz
- Laufzeit-/Ressourcenlimits

Typisches Schema, **nicht ungeprüft kopieren**:

```cron
*/5 * * * * /usr/local/bin/php /home/USER/domains/cloud.example.net/public_html/cron.php
```

Die realen DirectAdmin-Pfade haben Vorrang.

## 4. Upload-Limits

Große WebDAV-/Browseruploads hängen unter anderem von PHP- und Webserverlimits ab. Deshalb insbesondere prüfen:

- `upload_max_filesize`
- `post_max_size`
- `memory_limit`
- `max_execution_time`

Nach Änderungen einen echten großen Upload testen, nicht nur die PHP-Anzeige kontrollieren.

## 5. Betrieb und Updates

Vor jedem größeren Nextcloud-Update:

1. Kompatibilität der PHP-/Datenbankversion prüfen.
2. Backup erstellen.
3. Wartungsfenster einplanen.
4. nach dem Update Adminwarnungen, Cron und Dateizugriff prüfen.

Shared Hosting bedeutet, dass HostBrr PHP-/Datenbankumgebung und Teile der Serverkonfiguration kontrolliert. Deshalb kann ein zukünftiges Nextcloud-Release Anforderungen stellen, die nicht sofort verfügbar sind.

## 6. Backup

Mindestens sichern:

- `config/`
- Datenverzeichnis
- Datenbank
- ggf. zusätzliche Apps/Themes

Ein Backup **auf derselben StorageBox** schützt nicht vor Verlust/Suspendierung der gesamten Box. Für wichtige Nextcloud-Daten ist eine zusätzliche externe Kopie erforderlich.

Siehe [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md).

## 7. Restore-Test

Ein sinnvoller Restore-Test besteht nicht nur aus einer einzelnen Datei. Für eine Testinstanz bzw. ein Wartungsfenster sollte dokumentiert werden:

1. Datenbankdump wiederherstellen.
2. `config/` wiederherstellen.
3. Datenverzeichnis bereitstellen.
4. Dateirechte/Pfade prüfen.
5. Nextcloud isoliert starten.
6. Login, Dateien und Apps testen.
7. Cron prüfen.

Die genaue Restore-Prozedur hängt von der verwendeten Backupmethode ab.

## Sicherheit

- HTTPS erzwingen
- starke Admin-Zugangsdaten
- 2FA für Benutzer erwägen
- Adminzugang nicht für normale Nutzung verwenden
- Backups außerhalb derselben StorageBox halten
- Secrets nicht in dieser öffentlichen KB speichern

## Typische Fehler

### Große Uploads schlagen fehl

PHP-Limits und mögliche Webserverlimits prüfen. In HostBrr-Communityfällen waren insbesondere `upload_max_filesize` und `post_max_size` relevant.

### Background Jobs bleiben liegen

Cronmodus, Cronjob, PHP-CLI-Pfad, Ausführungsrechte und DirectAdmin-Cronlogs prüfen.

### Nextcloud meldet PHP-/OPcache-Warnungen

Nicht blind Warnungen unterdrücken. Prüfen, welche Einstellungen im Shared Hosting tatsächlich änderbar sind und welche HostBrr-seitig vorgegeben werden.

### Oberfläche funktioniert, aber alles ist langsam

Datenbank-/Metadatenlast, Cronrückstand, PHP-Limits und HDD-Charakter der StorageBox berücksichtigen. HostBrr positioniert die StorageBox nicht als datenbankoptimierten VPS.

## HostBrr-spezifisch noch zu testen

- angebotene Nextcloud-Version in Softaculous
- PHP-Versionen und Module
- Datenbankversion
- OPcache-Verhalten
- Cron alle 5 Minuten
- große WebDAV-Uploads
- viele kleine Dateien
- Update einer Testinstallation
- vollständiger Restore
- Unterschiede 2 TB vs. 8 TB
