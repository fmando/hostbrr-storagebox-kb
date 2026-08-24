---
title: "HostBrr Storage Box – How safe is it?"
source_type: forum
source_name: LowEndTalk
url: "https://lowendtalk.com/discussion/206272/hostbrr-storage-box-how-safe-is-it"
published: 2025-06
retrieved: 2026-08-24
reliability: community-plus-provider-comment
storagebox_generation: 2025
---

# Sicherheit und Verschlüsselung

Die Diskussion dreht sich um die Vertraulichkeit von Backups auf einem fremden Storage-System.

## Community-Konsens

Sensible Daten sollten vor dem Upload clientseitig verschlüsselt werden. Genannt werden unter anderem Restic, Borg, Cryptomator und rclone-basierte Verschlüsselung.

## Aussage des HostBrr-Betreibers

Der Betreiber empfiehlt in der Diskussion ebenfalls, persönliche bzw. sensible Informationen zu verschlüsseln.

## Schlussfolgerung für die KB

Unsere Standardempfehlung lautet: Bei Backups mit vertraulichen Inhalten Verschlüsselung vor dem Upload einsetzen und Schlüssel/Passphrase getrennt von der StorageBox sichern.

Transportverschlüsselung durch SSH/SFTP schützt die Übertragung, ersetzt aber keine Verschlüsselung der gespeicherten Daten gegenüber dem Storage-Anbieter.
