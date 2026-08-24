# Quellenregister

Hier werden externe Fundstellen gesammelt, bevor daraus Aussagen für die eigentliche Dokumentation übernommen werden.

## Quellenklassen

- `official/` – HostBrr-Webseite, Kundenbereich und offizielle Aussagen
- `forums/` – z. B. LowEndTalk, WebHostingTalk, NodeSeek
- `reddit/` – relevante Reddit-Diskussionen
- `howtos/` – externe technische Anleitungen

## Schema für einen Quelleintrag

```yaml
---
title: "Titel der Quelle"
source_type: forum
source_name: LowEndTalk
url: "https://..."
published: unknown
retrieved: 2026-08-24
reliability: community
storagebox_generation: unknown
location: unknown
---
```

Danach folgen Zusammenfassung, relevante technische Aussagen, Widersprüche zu anderen Quellen und Hinweise darauf, welche Punkte noch praktisch geprüft werden müssen.

Keine Passwörter, Tokens, privaten SSH-Keys oder sonstige Secrets in diesem Repository speichern.
