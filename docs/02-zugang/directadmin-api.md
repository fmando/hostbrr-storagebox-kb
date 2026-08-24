---
title: "DirectAdmin: API & Automatisierung"
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---
# DirectAdmin: API & Automatisierung

DirectAdmin bietet Programmierschnittstellen, mit denen sich Verwaltungsaufgaben automatisieren lassen. Welche Endpunkte und Aktionen einem HostBrr-StorageBox-Benutzer zur Verfügung stehen, hängt von Version und Accountrechten ab.

## Warum das interessant ist

Eine API könnte später beispielsweise helfen, Informationen für unsere KB oder eine eigene StorageBox-Verwaltung automatisch auszulesen:

- Account-/Quota-Informationen
- Domains
- Datenbanken
- Cronjobs
- Zertifikatsstatus
- weitere freigegebene Benutzerfunktionen

## API-Dokumentation

Offizielle DirectAdmin API-Dokumentation: https://docs.directadmin.com/developer/api/

DirectAdmin besitzt sowohl ältere CMD_API-Schnittstellen als auch neuere JSON-API-Funktionen. Für neue Automatisierung sollte zunächst geprüft werden, was die auf HostBrr eingesetzte Version tatsächlich anbietet.

## Authentifizierung

API-Zugangsdaten sind Secrets und dürfen nicht im Repository gespeichert werden.

Geeignete Ablageorte sind beispielsweise:

- Environment Variables
- Secret Manager
- lokal geschützte Konfigurationsdateien außerhalb des Git-Repositories

## Erst lesen, dann schreiben

Für eigene Tools sollten wir mit reinen Leseoperationen beginnen. Änderungen an Domains, Datenbanken oder Cronjobs werden erst automatisiert, wenn Verhalten und Berechtigungen verstanden sind.

## API-Version feststellen

Zunächst DirectAdmin-Version und sichtbare API-Dokumentation im Panel erfassen. Moderne Evolution-Versionen können eine interaktive API-Dokumentation anbieten.

## Beispielarchitektur

```text
StorageBox
   │
DirectAdmin API
   │
   ├── Inventarisierung
   ├── Quota-Monitoring
   ├── Zertifikatsstatus
   └── KB-/Monitoring-Integration
```

## Sicherheit

- eigenes API-/Login-Secret verwenden, wenn DirectAdmin dies unterstützt
- minimale Rechte
- Secrets nie committen
- HTTPS verwenden
- Logs auf versehentlich ausgegebene Credentials prüfen
- Schreiboperationen separat absichern

## HostBrr-Checkliste

- DirectAdmin-Version
- JSON API vorhanden?
- Live API Documentation sichtbar?
- Authentifizierungsoptionen
- read-only Token möglich?
- Quota-Endpunkt
- Domain-Endpunkte
- Cron-Endpunkte
- Datenbank-Endpunkte
- Rate Limits

## Weiterführende Dokumentation

- DirectAdmin API: https://docs.directadmin.com/developer/api/
- DirectAdmin Docs: https://docs.directadmin.com/
