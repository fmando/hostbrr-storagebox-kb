---
title: Verschlüsselung
category: sicherheit
status: maintained
last_reviewed: 2026-08-24
---
# Clientseitige Verschlüsselung

Für sensible Backups sollte die Verschlüsselung **vor dem Upload** stattfinden. Dann liegen auf der StorageBox nur verschlüsselte Daten.

## Geeignete Verfahren

### Restic

Restic verschlüsselt Repository-Daten standardmäßig und authentifiziert sie zusätzlich. Der Zugriff auf das Repository erfordert einen gültigen Repository-Key bzw. das zugehörige Passwort.

Wichtig: Geht das Passwort verloren und existiert kein weiterer gültiger Key, sind die Daten nicht mehr zugänglich.

Offizielle Dokumentation:

- https://restic.readthedocs.io/en/stable/070_encryption.html
- https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html

### BorgBackup

Borg unterstützt authentifizierte Verschlüsselung. Bei Borg 1.x kommen unter anderem `repokey`- bzw. `keyfile`-Varianten zum Einsatz; Borg 2 verwendet modernere AEAD-Modi. Für HostBrr muss deshalb immer zuerst die installierte Borg-Version festgestellt werden.

Offizielle Dokumentation:

- https://borgbackup.readthedocs.io/en/stable/
- https://borgbackup.readthedocs.io/en/latest/internals/security.html

### rclone crypt

`rclone crypt` legt eine Verschlüsselungsschicht über ein anderes Backend, etwa SFTP. Dateien werden lokal verschlüsselt und erst danach zur StorageBox übertragen.

Empfehlung: Zusätzlich zu `password` auch `password2` verwenden, damit eine benutzerdefinierte Salt-Komponente in die Schlüsselableitung eingeht.

Offizielle Dokumentation:

- https://rclone.org/crypt/

## Was Verschlüsselung nicht löst

Clientseitige Verschlüsselung schützt die **Vertraulichkeit**. Sie verhindert aber nicht automatisch:

- das Löschen verschlüsselter Backups,
- das Überschreiben durch einen kompromittierten Client,
- den Verlust des Schlüssels,
- fehlerhafte Retention-Regeln.

Dafür brauchen wir zusätzlich Versionierung, getrennte Berechtigungen und eine weitere unabhängige Kopie.

## Schlüssel sichern

Verschlüsselungsschlüssel und Passwörter gehören niemals in das Backup-Repository selbst oder unverschlüsselt in dieses Git-Repository. Sinnvoll sind mindestens zwei getrennte sichere Aufbewahrungsorte, z. B. Passwortmanager plus offline Recovery-Kopie.
