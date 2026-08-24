---
title: "Nextcloud and WebDAV on HostBrr DirectAdmin StorageBox"
source_type: forum
source_name: LowEndTalk
url: "https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p18"
published: 2025-12
retrieved: 2026-08-24
reliability: community
storagebox_generation: 2025
location: Germany/unknown
---

# Zusammenfassung

Diskussion zu WebDAV und Nextcloud auf DirectAdmin StorageBoxen.

## Relevante Aussagen

- Softaculous -> Nextcloud wird als üblicher Weg zu WebDAV beschrieben.
- Ein Nutzer meldete eine problematische globale Einstellung `opcache.restrict_api`, die Nextcloud-Warnungen verursachte und nicht per `.user.ini` überschrieben werden konnte.
- Andere Nutzer meldeten, Nextcloud bereits länger ohne dieses konkrete Problem zu betreiben.
- Dauerhaft laufende eigene Serverdienste wie MinIO sind aufgrund eingeschränkter Ports und Prozessumgebung problematisch.
- Bei Lastspitzen nach großen Verkaufsaktionen wurden zeitweise stark schwankende Transfergeschwindigkeiten gemeldet.

## Historischer WebDAV-Hinweis

In einem separaten HostBrr-Angebotsthread vom Februar 2025 wurde zunächst ein Limit bei großen WebDAV-Uploads gemeldet. Später meldete derselbe Nutzer, dass große Uploads wieder funktionierten. Providervertreter verwies bei sehr großen Dateien auf die in DirectAdmin einstellbaren PHP-Werte `upload_max_filesize` und `post_max_size`.

Quelle:

https://lowendtalk.com/discussion/202755/hostbrr-valentine-offers-10-gbps-storagebox-7-year-cpanel-85-off-flash/p4

## Einordnung

Nextcloud/WebDAV funktioniert grundsätzlich, aber die konkrete PHP-/OPcache-Konfiguration der jeweiligen StorageBox-Generation muss geprüft werden. Historische Fehler dürfen nicht ohne Test auf aktuelle Boxen übertragen werden.
