---
title: "HostBrr StorageBox – rclone VFS Cache und JuiceFS"
source_type: forum
source_name: LowEndTalk
retrieved: 2026-08-24
reliability: community
storagebox_generation: 2025-new-hardware
location: unknown
---
# rclone Cache und JuiceFS

## Fundstellen

- https://lowendtalk.com/discussion/202975/tutorial-turn-a-cheap-hdd-storage-vps-into-nvme-speed
- https://lowendtalk.com/discussion/212070/hostbrr-bf2025-deals-amd-threadripper-vps-15-year-1-tb-storage-just-1-month-more-inside/p12
- https://lowendtalk.com/discussion/205022/mounting-storage-box-on-a-vps

## Erkenntnisse

Mehrere Nutzer verwenden die StorageBox über rclone/SFTP als Mount und setzen lokalen VFS-Cache ein. Ein Tutorial demonstriert, wie lokale Cache-Schichten die vom Client wahrgenommene Schreibperformance massiv verändern können. Solche Cache-Benchmarks dürfen nicht mit der tatsächlichen Remote-Schreibgeschwindigkeit verwechselt werden.

Ein Nutzer mit einer rund 350-GB-Immich-Datensammlung berichtet von JuiceFS über SFTP mit lokalem NVMe-Cache. Als Vorteil gegenüber rclone mount nennt er schnellere Metadatenabfragen bei vielen kleinen Dateien. Derselbe Nutzer beschreibt rclone/SFTP weiterhin als sehr gut für direkten Dateizugriff und Transfers.

## Einordnung

- Für sequenzielle Backups bleibt rclone/SFTP die einfachere Lösung.
- Für Anwendungen mit sehr vielen Metadatenoperationen kann eine Cache-/Filesystem-Schicht interessant sein.
- Gemessene lokale Cache-Geschwindigkeit ist keine StorageBox-Netzwerk- oder HDD-Geschwindigkeit.
- Immich direkt auf einem entfernten FUSE-Mount ist ein fortgeschrittener Anwendungsfall und benötigt eigene Tests.
