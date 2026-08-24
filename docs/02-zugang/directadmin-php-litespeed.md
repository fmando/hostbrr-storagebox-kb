---
title: "DirectAdmin: PHP & LiteSpeed"
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---
# DirectAdmin: PHP & LiteSpeed

HostBrr bewirbt die StorageBox als Shared-Hosting-Umgebung mit PHP/LiteSpeed. Dadurch können Webanwendungen direkt auf der Box laufen. Gleichzeitig gelten die Ressourcen- und Sicherheitsgrenzen eines Shared-Hosting-Accounts.

## PHP-Version feststellen

Über SSH:

```bash
php -v
php -m
php --ini
```

Die CLI-Version muss nicht zwingend identisch mit der PHP-Version einer Domain sein.

## PHP-Einstellungen prüfen

Für Anwendungen sind unter anderem relevant:

- `memory_limit`
- `upload_max_filesize`
- `post_max_size`
- `max_execution_time`
- verfügbare Extensions
- OPcache

Über CLI:

```bash
php -i | grep -E 'memory_limit|upload_max_filesize|post_max_size|max_execution_time'
```

Bei Web-PHP kann die effektive Konfiguration abweichen.

## Mehrere PHP-Versionen

DirectAdmin kann je nach Serverkonfiguration mehrere PHP-Versionen bereitstellen. Welche Auswahl HostBrr für StorageBox-Kunden freigibt, muss am Account geprüft werden.

Offizielle DirectAdmin-Dokumentation: https://docs.directadmin.com/webservices/php/

## LiteSpeed

LiteSpeed ist ein Webserver mit Apache-Kompatibilität. Für unsere KB ist weniger die generische LiteSpeed-Administration wichtig als die Frage, welche Funktionen HostBrr dem Shared-Hosting-Benutzer zur Verfügung stellt.

Offizielle LiteSpeed-Dokumentation: https://docs.litespeedtech.com/

## Ressourcenlimits

Webanwendungen konkurrieren innerhalb des Accounts mit anderen Prozessen um die vom Hostingpaket bereitgestellten Ressourcen. Deshalb dokumentieren wir bei HostBrr getrennt:

- CPU-Limit
- RAM-Limit
- I/O-Limit
- IOPS
- Prozess-/Entry-Process-Limits, sofern sichtbar

Historische Community-Werte dürfen nicht ungeprüft auf aktuelle StorageBox-Generationen übertragen werden.

## Nextcloud-Relevanz

Vor einer Nextcloud-Installation sollten mindestens geprüft werden:

```bash
php -v
php -m
```

Danach werden die Ergebnisse mit den aktuellen Nextcloud-Systemanforderungen verglichen.

Nextcloud System Requirements: https://docs.nextcloud.com/server/latest/admin_manual/installation/system_requirements.html

## HostBrr-Checkliste

- PHP-Version(en) Web
- PHP-Version CLI
- PHP-Auswahl pro Domain?
- Extensions
- Memory Limit
- Upload Limit
- Execution Time
- OPcache
- LiteSpeed-Version, soweit sichtbar
- `.htaccess`-Verhalten
- eigene PHP-Einstellungen möglich?

## Weiterführende Dokumentation

- DirectAdmin PHP: https://docs.directadmin.com/webservices/php/
- LiteSpeed Docs: https://docs.litespeedtech.com/
- Nextcloud System Requirements: https://docs.nextcloud.com/server/latest/admin_manual/installation/system_requirements.html
