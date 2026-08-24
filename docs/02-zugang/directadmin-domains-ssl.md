---
title: "DirectAdmin: Domains, Subdomains und SSL"
category: zugang
status: official
last_reviewed: 2026-08-24
---

# Domains, Subdomains und SSL in DirectAdmin

Dieser Bereich ist relevant, wenn die HostBrr StorageBox nicht nur als SFTP-/Backupziel, sondern auch für Webanwendungen, Downloads, WebDAV oder Nextcloud verwendet wird.

## Brauche ich überhaupt eine Domain?

Für reines SFTP, rsync, Borg oder Restic ist eine eigene Domain normalerweise nicht erforderlich. Für Webdienste ist ein sprechender DNS-Name dagegen sinnvoll.

Beispiel:

```text
storage.example.de
cloud.example.de
files.example.de
```

## 1. Domain in DirectAdmin hinzufügen

DirectAdmin besitzt auf User-Level eine Domainverwaltung. Ob ein HostBrr-Tarif das Hinzufügen beliebig vieler Domains/Subdomains erlaubt, wird durch die Account-/Paketkonfiguration bestimmt.

Nach dem Hinzufügen muss DNS auf den von HostBrr vorgesehenen Zielhost zeigen.

## 2. DNS richtig setzen

Die konkreten Records hängen davon ab, ob HostBrr DNS selbst verwaltet oder ein externer DNS-Provider genutzt wird. Vor einem Zertifikatsantrag sollte der Name öffentlich korrekt auflösen.

Prüfen beispielsweise mit:

```bash
dig +short storage.example.de
```

oder:

```bash
nslookup storage.example.de
```

## 3. SSL Certificates öffnen

DirectAdmin unterstützt ACME-Zertifikate für Benutzer-Domains und Subdomains. Wenn die Funktion vom Provider aktiviert ist, findet sie sich auf User-Level unter **SSL Certificates**.

DirectAdmin kann Zertifikate automatisch ausstellen und erneuern. Die aktuelle Dokumentation beschreibt neben Let's Encrypt auch ZeroSSL als möglichen ACME-Provider. citeturn0search0

## 4. Zertifikat anfordern

Typischer Ablauf:

1. Domain/Subdomain anlegen.
2. DNS-Auflösung prüfen.
3. SSL Certificates öffnen.
4. ACME/Let's Encrypt auswählen.
5. gewünschte Namen auswählen.
6. Zertifikat anfordern.
7. HTTPS testen.

## 5. Automatische Erneuerung

DirectAdmin übernimmt bei aktivierter ACME-Funktion die Erneuerung automatisch. Ein eigener Certbot-Cronjob ist bei dieser Nutzung nicht erforderlich. citeturn0search0

## 6. HTTP auf HTTPS umleiten

DirectAdmin unterstützt eine serverseitige Force-Redirect-Funktion. Die offizielle Dokumentation weist darauf hin, dass dies gegenüber parallelen `.htaccess`-Weiterleitungen bedacht werden muss, um Schleifen zu vermeiden. citeturn0search0

## 7. Wildcard-Zertifikate

Wildcard-Zertifikate benötigen DNS-Validierung. DirectAdmin unterstützt im Evolution Skin auch die Konfiguration externer DNS-Provider für ACME/DNS-Challenges, sofern die Serverkonfiguration dies zulässt. Ob HostBrr diese Funktion für StorageBox-Accounts freigibt, muss praktisch geprüft werden. citeturn0search0

## 8. Typische Fehler

### Zertifikat kann nicht ausgestellt werden

Prüfen:

- zeigt DNS bereits korrekt?
- ist der Host öffentlich erreichbar?
- wurde der richtige Name ausgewählt?
- existiert bereits ein widersprüchlicher AAAA/A-Record?
- ist ACME/Let's Encrypt für den Account freigeschaltet?

### Browser zeigt falsches Zertifikat

DNS, Zielhost und die in DirectAdmin ausgewählte Domain prüfen. Bei Shared Hosting kann ein falscher Hostname dazu führen, dass das Zertifikat eines anderen Virtual Hosts bzw. das Serverzertifikat präsentiert wird.

## HostBrr-spezifische Fragen

Noch zu verifizieren:

- Anzahl erlaubter Domains/Subdomains je StorageBox
- HostBrr-DNS oder externe Nameserver?
- Let's Encrypt oder weitere ACME-Anbieter sichtbar?
- Wildcard/DNS-Provider im Evolution Skin verfügbar?
- Force HTTPS verfügbar?
- IPv6-Konfiguration der Webdienste

## Weiterführende Dokumentation

- [DirectAdmin – ACME for Domains](https://docs.directadmin.com/webservices/ssl/ssl-and-letsencrypt-for-domains.html)
- [DirectAdmin Knowledge Base](https://docs.directadmin.com/)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
