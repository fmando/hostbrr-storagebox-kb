---
title: "How can I mount a HostBrr Storage Box on Windows?"
source_type: forum
source_name: LowEndTalk
url: "https://lowendtalk.com/discussion/214012/how-can-i-mount-a-hostbrr-storage-box-on-windows"
published: 2026-01
retrieved: 2026-08-24
reliability: community
storagebox_generation: 2025-2026
---

# Windows-Mount

## Erfahrungswerte

- rclone wird als Mount-Lösung vorgeschlagen.
- Win-SSHFS wurde vom Threadersteller erfolgreich eingesetzt; er berichtet anschließend von guten Geschwindigkeiten.
- In diesem konkreten Fall war der fehlende SSH-Port die Ursache des Problems.
- Im Thread wird Port `53211` genannt, gleichzeitig wird ausdrücklich auf die individuelle Welcome-Mail verwiesen, falls der Port abweicht.

## KB-Regel

Port `53211` niemals pauschal als universellen HostBrr-Port dokumentieren. Immer Welcome-Mail bzw. aktuelle Zugangsdaten prüfen.

## Eigenes Howto geplant

Windows-Laufwerk über SSHFS mit separatem Abschnitt für rclone mount und automatischen Start.
